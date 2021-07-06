我用curl帮助我解决不少问题了，我打算整理下我对其的需求及用法：

1. curl -v google.com

   测试google.com是否可以正常访问，google.com域名是否正确解析。

2. curl --socks5-hostname 127.0.0.1:2000 google.com

   将域名解析的工作交给socks5服务器。

3. 很高级的知识，我的curl工具部分options用不了，我也想解决这个问题（暂时没有研究）
   
   - [打造你自己的cURL命令](https://cloud.tencent.com/developer/article/1626916)
   - [Problem running CURL with the dns option](https://unix.stackexchange.com/questions/588916/problem-running-curl-with-the-dns-option/588928#588928)
   - 