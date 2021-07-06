## 操作步骤

1. 指令如下：

~~~ shell

git config --global credential.helper store

sudo tee ~/.git-credentials <<-'EOF'
https://user:password@gitee.com
EOF

~~~

这个在只能走https协议的场景下非常好使，但是要记得将自己的账号设置成需要邮箱或手机号验证的，因为这种方案很容易泄漏账号信息。