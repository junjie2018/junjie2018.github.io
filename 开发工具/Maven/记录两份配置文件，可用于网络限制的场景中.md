## Maven配置代理及常用setting.xml文件

阿里源：

~~~ xml

<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">  

  <localRepository>F:\repository</localRepository>  

  <pluginGroups> </pluginGroups>  
 
  <proxies>
    <proxy>
      <id>my-proxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>127.0.0.1</host>
      <port>1080</port>
    </proxy>
  </proxies>  

  <servers></servers>  
 
  <mirrors> 
    <mirror>
        <id>aliyunmaven</id>
        <mirrorOf>*</mirrorOf>
        <name>ali public</name>
        <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>  
 
  <profiles></profiles>  
 
  <activeProfiles></activeProfiles> 
</settings>


~~~

默认源：

~~~ xml

<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <pluginGroups></pluginGroups>

  <localRepository>F:\repository</localRepository>  

  <proxies>
    <proxy>
      <id>my-proxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>127.0.0.1</host>
      <port>1080</port>
    </proxy>
  </proxies>  


  <servers></servers>

  <mirrors></mirrors>

  <profiles></profiles>

</settings>

~~~