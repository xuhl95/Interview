## web server

### 目录
- nginx
   - [nginx详解](https://www.cnblogs.com/ysocean/category/1289968.html)
      - [简介与安装](https://www.cnblogs.com/ysocean/p/9384877.html)
      - [nginx.conf 配置文件](https://www.cnblogs.com/ysocean/p/9384880.html)
      - [nginx 反向代理](https://www.cnblogs.com/ysocean/p/9392908.html)
      - [nginx 负载均衡](https://www.cnblogs.com/ysocean/p/9392912.html)
   - [Nginx 所使用的 epoll 模型是什么](https://www.toutiao.com/a6724547462964445704)
   - [Nginx面试中最常见的18道题](https://blog.csdn.net/Y0Q2T57s/article/details/88084000)
- apache
- [从输入URL到页面展示到底发生了什么](https://juejin.im/entry/5b44155f6fb9a04f932fdf80)
- nginx 负载均衡算法


### nginx负载均衡

#### Nginx负载均衡实现，有几种方式？

```
1、轮询（默认）
每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
upstream backserver {
    server 192.168.0.14;
    server 192.168.0.15;
}
2、weight
指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的 
情况。
upstream backserver {
    server 192.168.0.14 weight=3;
    server 192.168.0.15 weight=7;
}
权重越高，在被访问的概率越大，如上例，分别是30%，70%。
3、ip_hash
上述方式存在一个问题就是说，在负载均衡系统中，假如用户在某台服务器上登录了，那么该用户第二次请求的时候，因为我们是负载均衡系统，每次请求都会重新定位到服务器集群中的某一个，那么已经登录某一个服务器的用户再重新定位到另一个服务器，其登录信息将会丢失，这样显然是不妥的。
我们可以采用ip_hash指令解决这个问题，如果客户已经访问了某个服务器，当用户再次访问时，会将该请求通过哈希算法，自动定位到该服务器。
每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
upstream backserver {
    ip_hash;
    server 192.168.0.14:88;
    server 192.168.0.15:80;
}
4、fair（第三方）
按后端服务器的响应时间来分配请求，响应时间短的优先分配。
5、url_hash（第三方）
按访问url的hash结果来分配请求，使每个url定向到同一个（对应的）后端服务器，后端服务器为缓存时比较有效。
```