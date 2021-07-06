之所以研究这个技术，主要我通过EasyYapi导出到Postman的频率太高了，每次导出都需要重新填写Header，不是很舒服。我开发了如下脚本：

~~~

pm.request.headers.remove("Sdtc-Tenant-Id")
pm.request.headers.add({
    key: "token",
    value: "{{token}}"
})
pm.request.headers.add({
    key: "Sdtc-Tenant-Id",
    value: "12345678910"
})

~~~

## 参考资料

1. [Scripting with request data](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-data)