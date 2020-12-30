# 02-requests使用


安装requests：

```
pip install requests
```



## 一、发送http请求


使用requests库发送http请求，首先需要导入模块requests

```
import requests
```



### 发送get请求


使用requests发送get请求，只需要调用request.get方法即可：

```
r = requests.get("http://47.108.156.7:6001/get")
```

现在，我们有一个名为 r 的 Response 对象。我们可以从这个对象中获取所有我们想要的信息。


我们打印下输出，看下r：

```
r = requests.get("http://47.108.156.7:6001/get")
print(r)
返回结果：
<Response [200]>
```
可以发现是个Reponse对象。



Response对象常用的几个选项：

- status_code：返回HTTP状态码
- text：返回解码后的文本内容
- content：返回二进制内容的数据
- json：返回json格式的数据
- raw：服务器的原始套接字响应，需要确保在初始请求中设置了 stream=True，例如：r = requests.get('https://api.github.com/events', stream=True)
- url：返回请求的url（会拼接请求参数）
- herders：r.headers可以获取响应的头信息



**status_code使用：**

```
r = requests.get("http://47.108.156.7:6001/get")
print(r.status_code)  # http返回状态码

返回结果：
200

```

**text使用：**

```
r = requests.get("http://47.108.156.7:6001/get")
print(r.text)  # 自动解码后的返回结果


返回结果：
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "keep-alive", 
    "Host": "47.108.156.7:6001", 
    "User-Agent": "python-requests/2.25.0"
  }, 
  "origin": "171.223.206.218", 
  "url": "http://47.108.156.7:6001/get"
}
```

**content使用**

```
r = requests.get("http://47.108.156.7:6001/get")
print(r.content)  # 返回字节格式数据

返回结果：

b'{\n  "args": {}, \n  "headers": {\n    "Accept": "*/*", \n    "Accept-Encoding": "gzip, deflate", \n    "Connection": "keep-alive", \n    "Host": "47.108.156.7:6001", \n    "User-Agent": "python-requests/2.25.0"\n  }, \n  "origin": "171.223.206.218", \n  "url": "http://47.108.156.7:6001/get"\n}\n'
```


**json使用**

```
r = requests.get("http://47.108.156.7:6001/get")
print(r.json())  # 返回字节格式数据

返回结果：
{'args': {}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/get'}
```



接下来我们查看一下request get请求源码：

```
def get(url, params=None, **kwargs):
    r"""Sends a GET request.

    :param url: URL for the new :class:`Request` object.
    :param params: (optional) Dictionary, list of tuples or bytes to send
        in the query string for the :class:`Request`.
    :param \*\*kwargs: Optional arguments that ``request`` takes.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    """

    kwargs.setdefault('allow_redirects', True)
    return request('get', url, params=params, **kwargs)
```


可以发现：

- 第一个参数url：是一个url地址，用于请求http访问目标地址
- 第二个参数是params，用来发送请求的参数，默认值为None，可以用字典进行发送。
- 第三个参数是可变参数kwargs，用于接收key、value字典格式的参数。




### 发送带有参数的get请求


带有参数时，只需要在发送请求时，传递至params参数即可，如下：


```
payload = {'a': 1, "b": 2}
r = requests.get("http://47.108.156.7:6001/get", params=payload)
print(r.json())
```

返回结果如下：

{'args': {'a': '1', 'b': '2'}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Host': '47.108.156.7:6001', 'User-Agent': 'python-requests/2.25.0'}, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/get?a=1&b=2'}

可以发现args响应体里面就是我们传递的参数。


**注意：参数只能是字典格式。**



我们打印下r.url ，结果如下：

```
print(r.url)

返回结果如下：
http://47.108.156.7:6001/get?a=1&b=2
```
可以发现get请求，参数已经拼接在url地址上了。




HTTP请求，get方法参数都是query的形式，放在url路径后面进行拼接，这个是HTTP协议规定的。




### 请求添加header参数


如果需要在http请求头中添加信息，只需要传入headers即可，使用字典。如下：

```
 payload = {'a': 1, "b": '测试'}
 header = {"name": 'test'}
 r = requests.get("http://47.108.156.7:6001/get", payload, headers=header)
 print(r.json())

返回结果如下：

{'args': {'a': '1', 'b': '测试'}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'keep-alive', 'Host': '47.108.156.7:6001', 'Name': 'test', 'User-Agent': 'python-requests/2.25.0'}, 'origin': '171.223.206.218', 'url': 'http://47.108.156.7:6001/get?a=1&b=测试'}
```

可以从返回中发现：headers中有name参数，和我们传递的值test。





**总结**

- requests发送http get请求，第一个参数是url地址，如果需要参数传递，需传递给parms参数。
- 参数格式必须是字典形式。
- 添加header参数，参数以key-value的形式存储字典中，然后传递给形式参数headers即可。