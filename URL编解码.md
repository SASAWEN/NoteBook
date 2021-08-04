# URL编解码

1. URI解码
   1. 默认：IOS-8859-1
   2. 中文：URIEncoding=UTF-8
2. QueryString
   1. 默认：IOS-8859-1
   2. Header中ContentType定义的Charset：useBodyEncodingForURI="true"
3. Header
   1. 默认：IOS-8859-1
   2. 中文：用URIEncoding编码 -> 塞到header