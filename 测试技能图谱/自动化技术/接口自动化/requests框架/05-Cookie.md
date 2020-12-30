# 05-Cookie

如果某个响应中包含一些 cookie，你可以快速访问它们：


## 发送cookie


如果要发送cookir相关数据，可以使用cookies参数，如下：

```
cookies = dict(cookies_name='working')
r = requests.get("http://47.108.156.7:6001/get", cookies=cookies)
print(r.text)

返回结果：

{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "keep-alive", 
    "Cookie": "cookies_name=working", 
    "Host": "47.108.156.7:6001", 
    "User-Agent": "python-requests/2.25.0"
  }, 
  "origin": "171.223.206.218", 
  "url": "http://47.108.156.7:6001/get"
}

```



```
print(r.cookies)

返回结果如下：
<RequestsCookieJar[]>
```

Cookie 的返回对象为 RequestsCookieJar，它的行为和字典类似，但接口更为完整，适合跨域名跨路径使用。你还可以把 Cookie Jar 传到 Requests 中：


```
jar = requests.cookies.RequestsCookieJar()
jar.set('test_cookie', 'yum', domain='47.108.156.7:6001', path='/cookies')
r = requests.get("http://47.108.156.7:6001/cookies", cookies=jar)
print(r.text)
```





## 超时


你可以告诉 requests 在经过以 timeout 参数设定的秒数时间之后停止等待响应。基本上所有的生产代码都应该使用这一参数。如果不使用，你的程序可能会永远失去响应：

```
payload = {'a': 1, "b": 2}
r = requests.get("http://47.108.156.7:6001/get", payload, timeout=0.001)
print(r.json())

返回结果如下：
(Caused by ConnectTimeoutError(<urllib3.connection.HTTPConnection object at 0x10df17810>, 'Connection to 47.108.156.7 timed out. (connect timeout=0.001)'))
```

