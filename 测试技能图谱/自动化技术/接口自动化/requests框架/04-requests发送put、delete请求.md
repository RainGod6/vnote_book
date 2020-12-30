# 04-requests发送put、delete、options、head请求


发送put和delete请求，参数参照前面post，基本一致。


## 发送put请求



```
r = requests.post("http://47.108.156.7:6001/put", files={'file': open('test.py', 'rb')})
print(r.json())

返回结果如下：

{'args': {}, 'data': '', 'files': {}, 'form': {'key': 'value'}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Content-Length': '9', 'Content-Type': 'application/x-www-form-urlencoded', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'json': None, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/put'}
```


## 发送delete请求


```
r = requests.delete("http://47.108.156.7:6001/delete")
print(r.json())

返回结果如下：

{'args': {}, 'data': '', 'files': {}, 'form': {}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Content-Length': '0', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'json': None, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/delete'}
```