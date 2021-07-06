之前开发一个功能时需要用到yaml文件记录配置信息，刚开始使用snakeyml，但是snakeyml有个小小的问题，就是它解析后得到的map的key的顺序与文件中的并不一致。这种情况在很多需求下都是无所谓的，但是因为我们的功能恰巧很重视这个顺序，故只能放弃使用yml。（不知道snakeyml有没有办法通过配置保证key的顺序与文件中的一致）

后来偶尔发现SpringBoot提供了一些工具，可以简单的处理yaml文件，且能够保证key的顺序，记录如下：

~~~ java

import org.springframework.beans.factory.config.YamlPropertiesFactoryBean;
import org.springframework.core.io.ClassPathResource;

@SpringBootTest
class YAMLTest{

    @SuppressWarnings("Duplicates")
    public void execute() {
        YamlPropertiesFactoryBean factoryBean = new YamlPropertiesFactoryBean();
        factoryBean.setResources(new ClassPathResource("config/auto-create-tables-schedule.yml"));
        Properties properties = factoryBean.getObject();

        if (properties != null) {
            properties.forEach((table, tableCreateSql) -> {
                
            });
        }
    }

}

~~~

这个功能是很久前写的，但是最近因为又遇到了这个问题，故记录之，可能刚好记反了这两个yaml解析工具的特点，见谅，我下次遇到该问题时会及时更新该博客。