# 0.1 证书生成

## 生成密钥、证书
```python
# 生成服务器端私钥
openssl genrsa -out server.key 1024
# 生成服务器端公钥
openssl rsa -in server.key -pubout -out server.pem

# 生成客户端私钥
openssl genrsa -out client.key 1024
# 生成客户端公钥
openssl rsa -in client.key -pubout -out client.pem
```

## 生成 CA 证书

```python
# 生成 CA 私钥
openssl genrsa -out ca.key 1024

# X.509 Certificate Signing Request (CSR) Management.
openssl req -new -key ca.key -out ca.csr

# X.509 Certificate Data Management.
# 创建一个自当前日期起为期十年的根证书
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
```
### 第二步操作
```
root@iZm5eed3nhwqobcysjyzqtZ:~# openssl req -new -key ca.key -out ca.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:BeiJing
Locality Name (eg, city) []:BeiJing
Organization Name (eg, company) [Internet Widgits Pty Ltd]:jitongshidai
Organizational Unit Name (eg, section) []:jitongshidai
Common Name (e.g. server FQDN or YOUR name) []:dzq
Email Address []:1246747572@qq.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:不输入
An optional company name []:不输入

```

###  第三步操作
```
root@iZm5eed3nhwqobcysjyzqtZ:~# openssl x509 -req -days 3650 -in ca.csr -signkey ca.key -out ca.crt
Signature ok
subject=C = CN, ST = BeiJing, L = BeiJing, O = 00hall, OU = 00hall, CN = dzq, emailAddress = 1246747572@qq.com
Getting Private key
```

## 生成服务器端证书和客户端证书
```python
# 服务器端需要向 CA 机构申请签名证书，在申请签名证书之前依然是创建自己的 CSR 文件
openssl req -new -key server.key -out server.csr

# 向自己的 CA 机构申请证书，签名过程需要 CA 的证书和私钥参与，最终颁发一个带有 CA 签名的证书
openssl x509 -req -CA ../ca/ca.crt -CAkey ../ca/ca.key -CAcreateserial -in server.csr -out server.crt

# client 端
openssl req -new -key client.key -out client.csr

# client 端到 CA 签名
openssl x509 -req -CA ../ca/ca.crt -CAkey ../ca/ca.key -CAcreateserial -in client.csr -out client.crt
```

### 说明
自签名证书，可以不用生成client端



## 服务器端
```python
import os.path

from tornado import httpserver
from tornado import ioloop
from tornado import web

class TestHandler(web.RequestHandler):
    def get(self):
        self.write("Hello, World!")

def main():
    settings = {
        "static_path": os.path.join(os.path.dirname(__file__), "static"),
    }
    application = web.Application([
        (r"/", TestHandler),
    ], **settings)
    server = httpserver.HTTPServer(application, ssl_options={
           "certfile": os.path.join(os.path.abspath("."), "server.crt"),
           "keyfile": os.path.join(os.path.abspath("."), "server.key"),
    })
    server.listen(8000)
    ioloop.IOLoop.instance().start()

if __name__ == "__main__":
    main()
```

```js
// file http-server.js
var https = require('https');
var fs = require('fs');

var options = {
  key: fs.readFileSync('./keys/server.key'),
  cert: fs.readFileSync('./keys/server.crt')
};

https.createServer(options, function(req, res) {
  res.writeHead(200);
  res.end('hello world');
}).listen(8000);
```

## 客户端
### python版本
```python

import ssl

# 方式一：全局访问https
def fun1():
    ssl._create_default_https_context = ssl._create_unverified_context
    req = urllib2.Request(url="https://mmpkk.com:8000")
    res_data = urllib2.urlopen(req)#
    res = res_data.read()
    print res

# 方式二：SSLContext
def fun2():
    req = urllib2.Request(url="https://mmpkk.com:8000")
    context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2)
    res_data = urllib2.urlopen(req,context=context)
    res = res_data.read()
    print res
    
# 方式三：_create_unverified_context
def fun3():
    req = urllib2.Request(url="https://mmpkk.com:8000")
    context = ssl._create_unverified_context()
    res_data = urllib2.urlopen(req,context=context)
    res = res_data.read()
    print res

fun1()
fun2()
fun3()
```

### node.js版本
```js
// file http-client.js
var https = require('https');
var fs = require('fs');
process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";

var options = {
hostname: "mmpkk.com",
port: 8000,
path: '/',
methed: 'GET',
ca: [fs.readFileSync('./keys/ca.crt')
]
};
 
options.agent = new https.Agent(options);
 
var req = https.request(options, function(res) {
res.setEncoding('utf-8');
res.on('data', function(d) {
console.log(d);
});
});
req.end();
 
req.on('error', function(e) {
console.log(e);
});
```