## 常用指令

1. 常用指令

~~~ shell
sudo docker pull nginx:alpine
sudo docker tag nginx:alpine 192.168.30.174:80/test/nginx:alpine
sudo docker push 192.168.30.174:80/test/nginx:alpine

sudo docker tag jenkins/jenkins:lts  192.168.30.174:80/test/jenkins:lts
sudo docker push 192.168.30.174:80/test/jenkins:lts

sudo docker tag jenkins/jenkins:lts  192.168.30.174:80/test/jenkins:lts2
sudo docker push 192.168.30.174:80/test/jenkins:lts2

sudo docker tag cnych/jenkins:jnlp6 192.168.30.174:80/test/jenkins:jnlp6
sudo docker push 192.168.30.174:80/test/jenkins:jnlp6

sudo docker tag sonatype/nexus3:3.25.0 192.168.30.174:80/test/nexus3:3.25.0
sudo docker push 192.168.30.174:80/test/nexus3:3.25.0

sudo docker tag jenkinsci/jnlp-slave:latest 192.168.30.174:80/test/jnlp-slave:v1
sudo docker push 192.168.30.174:80/test/jnlp-slave:v1

sudo docker tag python:3.8.5 192.168.30.174:80/test/python:3.8.5
192.168.30.174:80/test/python:3.8.5

sudo docker tag busybox:latest 192.168.30.174:80/test/busybox:v1
sudo docker push 192.168.30.174:80/test/busybox:v1

sudo docker tag maven:alpine 192.168.30.174:80/test/maven:alpine
sudo docker push 192.168.30.174:80/test/maven:alpine

sudo docker tag google/cadvisor:latest 192.168.30.174:80/test/cadvisor:v1
sudo docker push 192.168.30.174:80/test/cadvisor:v1


sudo docker tag prom/prometheus:v2.1.0 192.168.30.174:80/test/prometheus:v2.1.0
sudo docker push 192.168.30.174:80/test/prometheus:v2.1.0
~~~

2. 常用指令2

~~~ shell
# junjie
sudo docker tag jenkins/inbound-agent:4.3-4 192.168.30.174:80/test/inbound-agent:4.3-4
sudo docker push 192.168.30.174:80/test/inbound-agent:4.3-4

# 虚拟机
sudo docker pull 192.168.30.174:80/test/inbound-agent:4.3-4
sudo docker tag 192.168.30.174:80/test/inbound-agent:4.3-4 jenkins/inbound-agent:4.3-4
~~~
