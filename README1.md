# python-tor
通过tor网络换ip进行爬虫


## 需要成功连接到tor网络 && window环境
-----------------------------------------
### 1.使用代码测试tor得到的ip地址
<pre><code>import socket
import socks
import requests
import os
socks.set_default_proxy(socks.SOCKS5, "127.0.0.1", 9050)
socket.socket = socks.socksocket
print(requests.get('http://api.ipify.org?format=json').text)
</code></pre>
##### 缺陷：想要换ip需要在vidalia中关闭再启动
------------------------------------------
### 2.让代码自动获得新ip
##### 2.1生成tor密码
>* ###### 把window下tor.exe所在文件夹添加到环境变量  
>* ###### 在命令行执行语句 tor --hash-password "password" （password为等下程序要用到的密码，要记住），会生成一串16:hash码下面用到   
>* ###### 编辑Vidalia.exe所在文件夹下的/Data/Tor/torrc文件 （用notepad打开），更改HashedControlPassword为上一句得出得hash值  

##### 2.2使用以下代码更换ip（最好2秒执行一次代码）
<pre><code>import requests
import socket
import socks
from stem import Signal  
from stem.control import Controller
url = 'http://api.ipify.org?format=json'
controller = Controller.from_port(port=9151)  #此处9151应该和Tor/torrc文件里面的ControlPort一致
controller.authenticate(password = 'abc97520')
controller.signal(Signal.NEWNYM)
socks.set_default_proxy(socks.SOCKS5, "127.0.0.1", 9050)
socket.socket = socks.socksocket
print(requests.get('http://api.ipify.org?format=json').text)
</code></pre>
##### 缺陷：在Linux中，即使用tor browser连接成功，没有vidalia也不能在Python切换ip,linux装vidalia非常麻烦，未成功。
-----------------------------------------------
