
### 使用`curl`命令发起请求
```
curl -s -v -- "www.baidu.com"        使用curl发起GET请求
```

```
curl -X post -s -v -- "www.baidu.com"       使用curl发起POST请求
```

会得到这样的以下两大段内容
```
* Rebuilt URL to: www.baidu.com/                                                                               
* Trying 14.215.177.39...                                                                                    
* TCP_NODELAY set                                                                                              
* Connected to www.baidu.com (14.215.177.39) port 80 (#0)                                                      
> GET / HTTP/1.1                                                                                               
> Host: www.baidu.com                                                                                          
> User-Agent: curl/7.55.1                                                                                      
> Accept: */*                                                                                                  
>                                                                                                              
< HTTP/1.1 200 OK                                                                                              
< Accept-Ranges: bytes                                                                                         
< Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform                                   
< Connection: keep-alive                                                                                       
< Content-Length: 2381                                                                                         
< Content-Type: text/html                                                                                      
< Date: Thu, 03 Jun 2021 14:18:13 GMT                                                                          
< Etag: "588604dc-94d"                                                                                         
< Last-Modified: Mon, 23 Jan 2017 13:27:56 GMT                                                                 
< Pragma: no-cache                                                                                             
< Server: bfe/1.0.8.18                                                                                         
< Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/                                            
<                                                                                                              
<!DOCTYPE html>                                                                                                
<!--STATUS OK--><html>……</html>                
* Connection #0 to host www.baidu.com left intact                                                              
```

```
* Rebuilt URL to: www.baidu.com/
* Trying 14.215.177.38...
* TCP_NODELAY set
* Connected to www.baidu.com (14.215.177.38) port 80 (#0)
> post / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.55.1
> Accept: */*
>
< HTTP/1.1 302 Found
< Content-Length: 17931
< Content-Type: text/html
< Date: Thu, 03 Jun 2021 14:43:56 GMT
< Etag: "54d9748e-460b"
< Server: bfe/1.0.8.18
<
<html>
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8">
<style data-for="result" id="css_result">
body{color:#333;background:#fff;padding:6px 0 0;margin:0;position:relative;min-width:900px}body,th,td,.p1,.p2{font-family:arial}p,form,ol,ul,li,dl,dt,dd,h3{margin:0;padding:0;list-style:none}input{padding-top:0;padding-bottom:0;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;box-sizing:border-box}table,img{border:0}td{font-size:9pt;line-height:18px}
...<span>此内容系百度根据您的指令自动搜索的结果，不代表
百度赞成被搜索网站的内容或立场</span></span>
<span id="help"><a href="http://www.baidu.com/search/jubao.html" target="_blank">举报</a></span></div>

<div class="c-tips-container" id="c-tips-container"></div></div></div><div class="c-tips-container" id="c-tips-container"></div>

</body></html>* Connection #0 to host www.baidu.com left intact
```
以上两大段内容中
- 带`*`的部分是注释
- 带`>`的部分是发出去请求
- 带`<`的部分是获得的响应

去除掉注释部分，我们可以看到请求与响应的基本格式
```
> GET / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.55.1
> Accept: */* 
> 

----

> post / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.55.1
> Accept: */*
>

```
#### 请求格式
```
> 动词 路径 协议/版本                  第一部分，请求行
> key1: value1                       第二部分，请求头部，由一对对键值对组成
> key2: value2
> Host: baidu.com
> User-Agent: curl/7.55.1
> Accept: */* 
>                                    第三部分，空行，只有一个换行符，用来区分第二四部分
>                                    第四部分，请求数据，可为空
```
1. 请求最多包含4部分，最少包含3部分，第4部分可为空
2. 其中第一部分的 “动词” 有
- `GET` 
- `POST` 
- `PUT`   全部更新替换
- `PATCH`  部分更新
- `DELETE`  删除
- `OPTION`
- `HEAD`
- `CONNECT`
- `TRACE`
3. 路径包含 “查询参数” ，但不包含锚点
4. 未写路径，默认为`/`根目录
5. 第二部分中的`content-Type`标注了第4部分的格式

#### 响应格式
```
< HTTP/1.1 302 Found
< Content-Length: 17931
< Content-Type: text/html
< Date: Thu, 03 Jun 2021 14:43:56 GMT
< Etag: "54d9748e-460b"
< Server: bfe/1.0.8.18
<
<html>
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8">
……
</html>
```
```
< 协议/版本 状态码 状态解释                       第一部分，状态行
< Content-Length: 17931                         第二部分，由键值对组成
< Content-Type: text/html
< Date: Thu, 03 Jun 2021 14:43:56 GMT
< Etag: "54d9748e-460b"
< Server: bfe/1.0.8.18
<                                               第三部分，空行
<html>                                          第四部分，响应正文
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8">
……
</html>
```
1. 第二部分的`Content-Type`标注了第4部分的格式，遵循MIME规范
2. 状态码
- `1XX` （消息）请求已被接受，需要请求者继续操作（不常用）
- `2XX` （成功）操作被成功接受并处理
- `3XX` （重定向）需要进一步操作以完成请求
- `4XX` （客户端错误）请求包含语法错误或者无法完成请求
- `5XX` （服务器错误）服务器处理请求过程中出现错误


| 常见请求 |                                                                              |
| -------- | ---------------------------------------------------------------------------- |
| 200      | 请求成功                                                                     |
| 204      | 服务器成功处理，但未返回内容。创建成功（POST请求）                           |
| 301      | 请求资源永久移到新的URL，新的URL在响应的Location域中返回                     |
| 302      | 资源临时移动，同样的新的临时URL在响应的Location域中返回                      |
| 304      | 所有请求资源未修改，服务器不会返回任何资源                                   |
| 403      | 服务器已理解请求，但是拒绝执行                                               |
| 404      | 请求失败，请求所希望得到的资源在服务器上并未找到                             |
| 500      | 服务器内部错误，无法完成请求                                                 |
| 503      | 服务器临时维护或过载，无法处理请求                                           |
| 502      | 作为网关或者代理工作的服务器尝试执行请求时，从远程上游服务器接收到无效的请求 |

#### 使用Chrome开发者工具查看 HTTP 请求和响应内容

1. 打开浏览器按快捷键`ctrl`+`shift`+`i`打开开发者工具页面
2. 点击`Network`
3. 在地址栏输入网址
![打开开发者工具输入网址.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23f8eda8d83a4fe597e48c5a7ac729f6~tplv-k3u1fbpfcp-watermark.image)
4. 查看`Request`再点击`View source`

![点request查看请求.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e2c06d3e064473cb5fc4529f0710a03~tplv-k3u1fbpfcp-watermark.image)
5. 就可以看到请求的前三部分，如果有请求的第四部分，在`FormData`或`Payload`里可以看到

![Request.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3925b9464c1a44a691f89e5aa6d304d1~tplv-k3u1fbpfcp-watermark.image)

![Response.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e4859f241a542ef96361217d56b6c9c~tplv-k3u1fbpfcp-watermark.image)
