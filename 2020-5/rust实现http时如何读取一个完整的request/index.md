### rust实现http时如何读取一个完整的request

用stream.read_to_end是不行的，tcpstream不是文件没有明确的结束符

需要先读取http header节，再找Content-Length header，然后读取body。

![](582229-20200522143415546-115427451.png)

这是http请求的结构

有个github可以参考，地址是[https://github.com/ltheinrich/lhi](https://github.com/ltheinrich/lhi)
