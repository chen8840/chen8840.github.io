### SpringBoot使用CORS的addCorsMappings中会遇到的问题

跨域需要后端需要设置响应的跨域头

如下

```java
public void addCorsMappings(CorsRegistry registry) {

    registry.addMapping("/**")
            .allowedOrigins("*")
            .allowedMethods("POST", "GET", "PUT", "OPTIONS", "DELETE")
            .allowCredentials(true)
            .allowedHeaders("*")
            .maxAge(3600);
}
```

同时项目需要验证request的header中附带的token验证信息，增加token验证拦截器

此时跨域头设置会失效，原因是 当请求到来时会先进入拦截器中，而不是进入Mapping映射中，所以返回的头信息中并没有配置的跨域信息。

浏览器遇到跨域请求会先发送options请求，而options请求是不会携带自定义头的（比如token），此时请求先进入token拦截器，验证失败并返回响应，绕过了Mapping映射，跨域失败。浏览器后续的请求不会发送。
