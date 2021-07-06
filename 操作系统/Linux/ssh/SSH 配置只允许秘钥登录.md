## 操作步骤

1. 如下代码生成秘钥对，可以一致按回车，使用默认值（我设置了密码）

~~~

ssh-keygen

~~~

2. 执行如下指令，将生成的pub秘钥添加到信任名单，并检查文件权限正确（权限太高，无法正常登录）

~~~

cd .ssh
cat id_rsa.pub >> authorized_keys

chmod 600 authorized_keys
chmod 700 ~/.ssh

~~~

3. 通过/etc/ssh/sshd_config文件配置ssh服务器）（我服务器上没有找到RSAAuthentication，我只配置了PubkeyAuthentication），完成配置后，使用service sshd restart指令重启ssh服务。

~~~

RSAAuthentication yes
PubkeyAuthentication yes

service sshd restart

~~~

4. 完成配置后，可以尝试用秘钥登录一次，具体操作为：下载id_rsa文件到本地，然后导入到XShell中，在登录的时候选择秘钥登录，然后设置秘钥的密码，就可以完成登录了。

5. 完成登录后，配置ssh服务器，禁止用户用密码登录。

~~~

PasswordAuthentication no

service sshd restart

~~~

## 参考教程

1. [设置 SSH 通过密钥登录](https://www.runoob.com/w3cnote/set-ssh-login-key.html)