#Requests Library

[摘自request库官方文档](https://requests.readthedocs.io/en/master/user/quickstart/)&nbsp;&nbsp;&nbsp;author:高梓源&nbsp;&nbsp;&nbsp;date:1.26

---

[TOC]

##发出请求

首先导入request模块：

```python
>>> import requests
```

利用请求获取网页：

```python
>>> r = requests.get('https://api.github.com/events')
```

r为response类的对象，包括content, cookies, encoding, headers, json, text, url等域

上面示例为HTTP的get方法，其他方法同理：

```python
>>> r = requests.post('https://httpbin.org/post', data = {'key':'value'})
>>> r = requests.put('https://httpbin.org/put', data = {'key':'value'})
>>> r = requests.delete('https://httpbin.org/delete')
>>> r = requests.head('https://httpbin.org/get')
>>> r = requests.options('https://httpbin.org/get')
```

注意到post等方法都通过data传参，而get方法需要显式地将参数放在url中，通过`get?key=val`的形式，在python可以通过参数字典赋值给get方法params参数实现：

```python
>>> payload = {'key1': 'value1', 'key2': 'value2'}
>>> r = requests.get('https://httpbin.org/get', params=payload)
#可以检验get命令url是否正确
>>> print(r.url)
https://httpbin.org/get?key2=value2&key1=value1
#甚至可以列表传参？正常人不会这么写php吧，注入问题
>>> payload = {'key1': 'value1', 'key2': ['value2', 'value3']}
```

##POST请求

定义一个参数字典，传递给post方法data参数即可，data参数可以以元组和字典+List方式呈现

```python
>>> payload = {'key1': 'value1', 'key2': 'value2'}
>>> payload_tuples = [('key1', 'value1'), ('key1', 'value2')]
>>> payload_dict = {'key1': ['value1', 'value2']}

>>> r = requests.post("https://httpbin.org/post", data=payload)
>>> r1 = requests.post('https://httpbin.org/post', data=payload_tuples)
>>> r2 = requests.post('https://httpbin.org/post', data=payload_dict)
```

也可以传递参数给json字段`>>> r = requests.post(url, json=payload)`

##响应内容

利用response对象的text域可以读取响应内容：

```python
>>> r.text
'[{"repository":{"open_issues":0,"url":"https://github.com/...

>>> r.encoding
ISO-8859-1
```

##响应头

通过`r.headers`查看，查询单个字段大小写不重要

##自定义响应头

定义一个字典header，传递到get方法header参数即可

```python
>>> url = 'https://api.github.com/some/endpoint'
>>> headers = {'user-agent': 'my-app/0.0.1'}
>>> r = requests.get(url, headers=headers)
```


##响应状态码
通过`r.status_code`查看，200-299正常，400-499客户端错误，500-599服务器错误