我之前使用的方式是一行一行的读取文件，然后调用update方法，需要写好多行代码，下面的写法更简洁一些：

~~~ python

import hashlib

fd=open("1.jpg","r")
fcont=fd.read()
fmd5=hashlib.md5(fcont)  
print fmd5.hexdigest() #get 32 value

~~~