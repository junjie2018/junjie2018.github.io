我注意到MyBatis-Plus的配置文件中有如下配置：

~~~ yml

mybatis-plus:
  mapper-locations: classpath:mapper/*Mapper.xml

~~~

我觉得在配置文件中使用`classpath:`表示当前项目的资源文件是一件非常优雅的编码方式，我也想在我自己的配置项中使用这种写法。但是当我发现将`classpath:`应用到自己的配置项时发现，根本就没有自动将classpath:转换成资源目录，断点如下：

![2021-06-28-10-35-42](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-10-35-42.png)

针对这个问题，我其实开发了自己的解决方案，代码如下：

~~~ java

    public String getTemplateDir() {

        return templateDir.startsWith("classpath:") ?
                classpathLabelToAbsolute(templateDir) :
                templateDir;

    }

    public String getTableDataDir() {

        return tableDataDir.startsWith("classpath:") ?
                classpathLabelToAbsolute(tableDataDir) :
                tableDataDir;

    }

    private String classpathLabelToAbsolute(String path) {
        try {
            return ResourceUtils.getFile(templateDir).getAbsolutePath();
            // 以后验证一下这个方法
            // return this.getClass().getClassLoader().getResource(templateDir).getFile().toString()
        } catch (IOException e) {
            throw new RuntimeException("TemplateDir Config Wrong");
        }
    }

~~~

但是我觉得代码并不是很优雅，所以我决定参考一下MyBatis-Plus对此的实现。

## MyBatis-Plus

### 寻找MyBatis-Plus的配置类：

![2021-06-28-11-05-02](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-11-05-02.png)

我这次寻找配置类，完全是靠运气，一下子就找到了，我想规则大概就是配置类都放放在starter中的吧。

### MyBatis-Plus的实现

MyBatis-Plus的处理代码如下：

~~~ java

    private static final ResourcePatternResolver resourceResolver = new PathMatchingResourcePatternResolver();


    public Resource[] resolveMapperLocations() {
        return Stream.of(Optional.ofNullable(this.mapperLocations).orElse(new String[0]))
            .flatMap(location -> Stream.of(getResources(location)))
            .toArray(Resource[]::new);
    }

    private Resource[] getResources(String location) {
        try {
            return resourceResolver.getResources(location);
        } catch (IOException e) {
            return new Resource[0];
        }
    }

~~~

我们的代码除列使用的工具类不一样，其他的思路基本是一致的。我看过MyBatis-Plus使用的工具类源码，挺复杂的，暂时不想深入研究。

## Spring Datasource

我注意到Spring Datasource也是支持`classpath:`配置的，我就好奇它是怎么实现对该配置的支持。

~~~ yaml

spring:
  datasource:
    schema: classpath:h2/schema.sql
    data: classpath:h2/data.sql

~~~

### 找到Spring Datasource配置类

我这次鸟枪换泡，用更高级更强大的方法找到了配置类（实际上是因为我一个一个文件翻找，找了半天都找不到），思路如下：

1. 先在Idea中下载下载所有Maven依赖的源码
2. 使用全局搜索`spirng.datasource`

![2021-06-28-11-50-26](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-11-50-26.png)

### Spring DataSource中的写法

代码如下，我没有深度分析，直接贴过来了：

~~~ java

	boolean createSchema() {
		List<Resource> scripts = getScripts("spring.datasource.schema", this.properties.getSchema(), "schema");
		if (!scripts.isEmpty()) {
			if (!isEnabled()) {
				logger.debug("Initialization disabled (not running DDL scripts)");
				return false;
			}
			String username = this.properties.getSchemaUsername();
			String password = this.properties.getSchemaPassword();
			runScripts(scripts, username, password);
		}
		return !scripts.isEmpty();
	}

    	private List<Resource> getScripts(String propertyName, List<String> resources, String fallback) {
		if (resources != null) {
			return getResources(propertyName, resources, true);
		}
		String platform = this.properties.getPlatform();
		List<String> fallbackResources = new ArrayList<>();
		fallbackResources.add("classpath*:" + fallback + "-" + platform + ".sql");
		fallbackResources.add("classpath*:" + fallback + ".sql");
		return getResources(propertyName, fallbackResources, false);
	}

	private List<Resource> getResources(String propertyName, List<String> locations, boolean validate) {
		List<Resource> resources = new ArrayList<>();
		for (String location : locations) {
			for (Resource resource : doGetResources(location)) {
				if (resource.exists()) {
					resources.add(resource);
				}
				else if (validate) {
					throw new InvalidConfigurationPropertyValueException(propertyName, resource,
							"The specified resource does not exist.");
				}
			}
		}
		return resources;
	}

	private Resource[] doGetResources(String location) {
		try {
			SortedResourcesFactoryBean factory = new SortedResourcesFactoryBean(this.resourceLoader,
					Collections.singletonList(location));
			factory.afterPropertiesSet();
			return factory.getObject();
		}
		catch (Exception ex) {
			throw new IllegalStateException("Unable to load resources from " + location, ex);
		}
	}

~~~

## 小结

我最终肯定还是选择我自己的方案，因为我主要用于工具包的开发，使用我这种方案更易读（其实我觉得MyBatis-Plus也挺好的）。另外使用`ResourceUtils`貌似打包成Jar包后无法正确的读取路径，我并没验证这个问题。

![2021-06-28-11-57-21](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-11-57-21.png)

我决定当我在生产环境中需要使用相关技术时我再继续探索这个解决方案。

## 参考资料

1. [SpringBoot读取resources目录下的文件](https://juejin.cn/post/6844903962328432648)
2. [springboot获取资源文件、编译文件路径（打包后）](https://blog.csdn.net/qq_36441163/article/details/106026117)
3. [Spring 应用访问Classpath路径下的文件](https://blog.csdn.net/neweastsun/article/details/100591046)