## 操作步骤

1. https://jdk.java.net/java-se-ri/7下载openJDK 7安装包

![2020-08-14-12-28-36](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-08-14-12-28-36.png)

2. 解压，设置环境变量

~~~ shell

tar -zxvf openjdk-7u75-b13-linux-x64-18_dec_2014.tar.gz

sudo tee -a /etc/profile <<-'EOF'
JAVA_HOME=/home/vagrant/software/openjdk/java-se-7u75-ri
PATH=${JAVA_HOME}/bin:${PATH}
EOF
source /etc/profile

~~~