下载文件中包含%符号的时候，比如 http://www.baidu.com/some/path/发%.zip， 这时直接用 <a href="http://www.baidu.com/some/path/发%.zip">下载</a> 这样的链接形式是不行的，因为 % 符号是特殊字符，浏览器会对其他特殊字符（比如中文）进行编码，但是对于%号，需要手动进行转义。

正确做法是使用encodeURI对http://www.baidu.com/some/path/发%.zip进行转义让他变成http://www.baidu.com/some/path/%E5%8F%91%25.zip，然后再放到链接中。
