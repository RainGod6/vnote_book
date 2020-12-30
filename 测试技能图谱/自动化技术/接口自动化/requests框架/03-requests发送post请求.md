# 03-requests发送post请求



## 发送post请求


使用requests发送post请求，也相当简单。如下：

```
r = requests.post("http://47.108.156.7:6001/post")
print(r.json())

返回结果如下：
{'args': {}, 'data': '', 'files': {}, 'form': {}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Content-Length': '0', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'json': None, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/post'}
```



### 发送query参数

发送query参数和get请求一致，如下：

```
payload = {'a': 1, "c": 'ccccc'}
r = requests.post("http://47.108.156.7:6001/post", params=payload)
print(r.json())

返回结果：
{'args': {'a': '1', 'c': 'ccccc'}, 'data': '', 'files': {}, 'form': {}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Content-Length': '0', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'json': None, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/post?a=1&c=ccccc'}

```



### 发送form表单形式参数


发送form表单的参数，传递的是字典格式的数据，需要传递至data形式参数即可。如下：

```
payload = {'a': 1, "c": 'ccccc'}
data = {'name': '测试', 'pwd': 'www111'}
r = requests.post("http://47.108.156.7:6001/post", params=payload, data=data)
print(r.json())

返回结果：

{'args': {'a': '1', 'c': 'ccccc'}, 'data': '', 'files': {}, 'form': {'name': '测试', 'pwd': 'www111'}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Content-Length': '34', 'Content-Type': 'application/x-www-form-urlencoded', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'json': None, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/post?a=1&c=ccccc'}
```

可以发现，返回的内容中form有传递的参数。



### 发送json格式参数


发送json格式的参数，传递的是json的数据格式，需要传递至json形式参数即可。如下：

```
payload = {'a': 1, "c": 'ccccc'}
data = [
{
        'id': 1,
        'name': '张三',
        'age': 22
    },
{
        'id': 2,
        'name': '李四',
        'age': 23
    }
]

json_data = {'code': 00, 'list': data}
r = requests.post("http://47.108.156.7:6001/post", params=payload, json=json_data)
print(r.json())

返回结果如下：

{'args': {'a': '1', 'c': 'ccccc'}, 'data': '{"code": 0, "list": [{"id": 1, "name": "\\u5f20\\u4e09", "age": 22}, {"id": 2, "name": "\\u674e\\u56db", "age": 23}]}', 'files': {}, 'form': {}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Content-Length': '113', 'Content-Type': 'application/json', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'json': {'code': 0, 'list': [{'age': 22, 'id': 1, 'name': '张三'}, {'age': 23, 'id': 2, 'name': '李四'}]}, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/post?a=1&c=ccccc'}
```



### 发送文件形式参数

发送file格式的参数，传递的是file文件，需要传递至file形式参数即可。如下：

```
r = requests.post("http://47.108.156.7:6001/post", files={'file': open('test.py', 'rb')})
print(r.json())

返回结果如下：

{'args': {}, 'data': '', 'files': {'file': 'a = 1'}, 'form': {}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Content-Length': '148', 'Content-Type': 'multipart/form-data; boundary=32d837d17d5a53fa6c5ed8bd02aa91e0', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'json': None, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/post'}
```


可以发现返回的结果中，files内容包含文件中的内容。


