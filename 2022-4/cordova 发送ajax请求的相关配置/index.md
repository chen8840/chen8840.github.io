### cordova 发送ajax请求的相关配置

1. \<access origin="*" /\>
2. \<preference name="scheme" value="http" /\> 这里让cordova webview使用http协议链接本地文件，如果需要访问http协议的后台服务需要改下这里，如果访问https协议的后台服务不需要。
3. <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: http: https: 'unsafe-eval' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; media-src *; script-src 'self' 'unsafe-inline' 'unsafe-eval';img-src 'self' data: content:;"\> default-src是与ajax相关的
4. 服务端设置下Access-Control-Allow-Origin为http://localhost和https://localhost
