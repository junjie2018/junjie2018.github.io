## 操作步骤

1. 指令如下

~~~ shell

mkdir -p ~/.pip

sudo tee ~/.pip/pip.conf <<-'EOF'
sudo tee 
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
EOF

~~~