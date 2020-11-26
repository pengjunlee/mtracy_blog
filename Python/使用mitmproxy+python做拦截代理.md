本文是一个较为完整的 mitmproxy 教程，侧重于介绍如何开发拦截脚本，帮助读者能够快速得到一个自定义的代理工具。

本文假设读者有基本的 python 知识，且已经安装好了一个 python 3 开发环境。如果你对 nodejs 的熟悉程度大于对 python，可移步到 anyproxy，anyproxy 的功能与 mitmproxy 基本一致，但使用 js 编写定制脚本。除此之外我就不知道有什么其他类似的工具了，如果你知道，欢迎评论告诉我。

本文基于 mitmproxy v4，当前版本号为 v4.0.1。

mitmproxy 是什么


顾名思义，mitmproxy 就是用于 MITM 的 proxy，MITM 即中间人攻击（Man-in-the-middle attack）。用于中间人攻击的代理首先会向正常的代理一样转发请求，保障服务端与客户端的通信，其次，会适时的查、记录其截获的数据，或篡改数据，引发服务端或客户端特定的行为。

不同于 fiddler 或 wireshark 等抓包工具，mitmproxy 不仅可以截获请求帮助开发者查看、分析，更可以通过自定义脚本进行二次开发。举例来说，利用 fiddler 可以过滤出浏览器对某个特定 url 的请求，并查看、分析其数据，但实现不了高度定制化的需求，类似于：“截获对浏览器对该 url 的请求，将返回内容置空，并将真实的返回内容存到某个数据库，出现异常时发出邮件通知”。而对于 mitmproxy，这样的需求可以通过载入自定义 python 脚本轻松实现。

但 mitmproxy 并不会真的对无辜的人发起中间人攻击，由于 mitmproxy 工作在 HTTP 层，而当前 HTTPS 的普及让客户端拥有了检测并规避中间人攻击的能力，所以要让 mitmproxy 能够正常工作，必须要让客户端（APP 或浏览器）主动信任 mitmproxy 的 SSL 证书，或忽略证书异常，这也就意味着 APP 或浏览器是属于开发者本人的——显而易见，这不是在做黑产，而是在做开发或测试。

那这样的工具有什么实际意义呢？据我所知目前比较广泛的应用是做仿真爬虫，即利用手机模拟器、无头浏览器来爬取 APP 或网站的数据，mitmproxy 作为代理可以拦截、存储爬虫获取到的数据，或修改数据调整爬虫的行为。

事实上，以上说的仅是 mitmproxy 以正向代理模式工作的情况，通过调整配置，mitmproxy 还可以作为透明代理、反向代理、上游代理、SOCKS 代理等，但这些工作模式针对 mitmproxy 来说似乎不大常用，故本文仅讨论正向代理模式。

安装
“安装 mitmproxy”这句话是有歧义的，既可以指“安装 mitmproxy 工具”，也可以指“安装 python 的 mitmproxy 包”，注意后者是包含前者的。

如果只是拿 mitmproxy 做一个替代 fiddler 的工具，没有什么定制化的需求，那完全只需要“安装 mitmproxy 工具”即可，去 mitmproxy 官网 上下载一个 installer 便可开箱即用，不需要提前准备好 python 开发环境。但显然，这不是这里要讨论的，我们需要的是“安装 python 的 mitmproxy 包”。

安装 python 的 mitmproxy 包除了会得到 mitmproxy 工具外，还会得到开发定制脚本所需要的包依赖，其安装过程并不复杂。

首先需要安装好 python，版本需要不低于 3.6，且安装了附带的包管理工具 pip。不同操作系统安装 python 3 的方式不一，参考 python 的下载页，这里不做展开，假设你已经准备好这样的环境了。

安装开始。

在 linux 中：

sudo pip3 install mitmproxy
在 windows 中，以管理员身份运行 cmd 或 power shell：

pip3 install mitmproxy
安装结束后，系统将拥有 mitmproxy、mitmdump、mitmweb 三个命令，由于 mitmproxy 命令不支持在 windows 系统中运行（这没关系，不用担心），我们可以拿 mitmdump 测试一下安装是否成功，执行：

mitmdump --version
应当可以看到类似于这样的输出：

Mitmproxy: 4.0.1
Python:    3.6.5
OpenSSL:   OpenSSL 1.1.0h  27 Mar 2018
Platform:  Windows-10-10.0.16299-SP0
运行
要启动 mitmproxy 用 mitmproxy、mitmdump、mitmweb 这三个命令中的任意一个即可，这三个命令功能一致，且都可以加载自定义脚本，唯一的区别是交互界面的不同。

mitmproxy 命令启动后，会提供一个命令行界面，用户可以实时看到发生的请求，并通过命令过滤请求，查看请求数据。形如：



mitmweb 命令启动后，会提供一个 web 界面，用户可以实时看到发生的请求，并通过 GUI 交互来过滤请求，查看请求数据。形如：



mitmdump 命令启动后——你应该猜到了，没有界面，程序默默运行，所以 mitmdump 无法提供过滤请求、查看数据的功能，只能结合自定义脚本，默默工作。

由于 mitmproxy 命令的交互操作稍显繁杂且不支持 windows 系统，而我们主要的使用方式又是载入自定义脚本，并不需要交互，所以原则上说只需要 mitmdump 即可，但考虑到有交互界面可以更方便排查错误，所以这里以 mitmweb 命令为例。实际使用中可以根据情况选择任何一个命令。

启动 mitmproxy：

mitmweb
应当看到如下输出：

Web server listening at http://127.0.0.1:8081/
Proxy server listening at http://*:8080
mitmproxy 绑定了 *:8080 作为代理端口，并提供了一个 web 交互界面在 127.0.0.1:8081。

现在可以测试一下代理，让 Chrome 以 mitmproxy 为代理并忽略证书错误。为了不影响平时正常使用，我们不去改 Chrome 的配置，而是通过命令行带参数起一个 Chrome。如果你不使用 Chrome 而是其他浏览器，也可以搜一下对应的启动参数是什么，应该不会有什么坑。此外示例仅以 windows 系统为例，因为使用 linux 或 mac 开发的同学应该更熟悉命令行的使用才对，应当能自行推导出在各自环境中对应的操作。

由于 Chrome 要开始赴汤蹈火走代理了，为了方便继续在 web 界面上与 mitmproxy 交互，我们委屈求全使用 Edge 或其他浏览器打开 127.0.0.1:8081。插一句，我用 Edge 实在是因为机器上没其他浏览器了（IE 不算），Edge 有一个默认禁止访问回环地址的狗屁设定，详见解决方案。

接下来关闭所有 Chrome 窗口，否则命令行启动时的附加参数将失效。打开 cmd，执行：

"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --proxy-server=127.0.0.1:8080 --ignore-certificate-errors
前面那一长串是 Chrome 的的安装路径，应当根据系统实际情况修改，后面两参数设置了代理地址并强制忽略掉证书错误。用 Chrome 打开一个网站，可以看到：



同时在 Edge 上可以看到：



脚本
完成了上述工作，我们已经具备了操作 mitmproxy 的基本能力 了。接下来开始开发自定义脚本，这才是 mitmproxy 真正强大的地方。

脚本的编写需要遵循 mitmproxy 规定的套路，这样的套路有两个，使用时选其中一个套路即可。

第一个套路是，编写一个 py 文件供 mitmproxy 加载，文件中定义了若干函数，这些函数实现了某些 mitmproxy 提供的事件，mitmproxy 会在某个事件发生时调用对应的函数，形如：

import mitmproxy.http
from mitmproxy import ctx
 
num = 0
 
 
def request(flow: mitmproxy.http.HTTPFlow):
    global num
    num = num + 1
    ctx.log.info("We've seen %d flows" % num)
 
第二个套路是，编写一个 py 文件供 mitmproxy 加载，文件定义了变量 addons，addons 是个数组，每个元素是一个类实例，这些类有若干方法，这些方法实现了某些 mitmproxy 提供的事件，mitmproxy 会在某个事件发生时调用对应的方法。这些类，称为一个个 addon，比如一个叫 Counter 的 addon：

import mitmproxy.http
from mitmproxy import ctx
 
 
class Counter:
    def __init__(self):
        self.num = 0
 
    def request(self, flow: mitmproxy.http.HTTPFlow):
        self.num = self.num + 1
        ctx.log.info("We've seen %d flows" % self.num)
 
 
addons = [
    Counter()
]
这里强烈建议使用第二种套路，直觉上就会感觉第二种套路更为先进，使用会更方便也更容易管理和拓展。况且这也是官方内置的一些 addon 的实现方式。

我们将上面第二种套路的示例代码存为 addons.py，再重新启动 mitmproxy：

mitmweb -s addons.py
当浏览器使用代理进行访问时，就应该能看到控制台里有类似这样的日志：

Web server listening at http://127.0.0.1:8081/
Loading script addons.py
Proxy server listening at http://*:8080
We've seen 1 flows
……
……
We've seen 2 flows
……
We've seen 3 flows
……
We've seen 4 flows
……
……
We've seen 5 flows
……
这就说明自定义脚本生效了。

事件
上述的脚本估计不用我解释相信大家也看明白了，就是当 request 发生时，计数器加一，并打印日志。这里对应的是 request 事件，那拢共有哪些事件呢？不多，也不少，这里详细介绍一下。

事件针对不同生命周期分为 5 类。“生命周期”这里指在哪一个层面看待事件，举例来说，同样是一次 web 请求，我可以理解为“HTTP 请求 -> HTTP 响应”的过程，也可以理解为“TCP 连接 -> TCP 通信 -> TCP 断开”的过程。那么，如果我想拒绝来个某个 IP 的客户端请求，应当注册函数到针对 TCP 生命周期 的 tcp_start 事件，又或者，我想阻断对某个特定域名的请求时，则应当注册函数到针对 HTTP 声明周期的 http_connect 事件。其他情况同理。

下面一段估计会又臭又长，如果你没有耐心看完，那至少看掉针对 HTTP 生命周期的事件，然后跳到示例。

1. 针对 HTTP 生命周期
def http_connect(self, flow: mitmproxy.http.HTTPFlow):
(Called when) 收到了来自客户端的 HTTP CONNECT 请求。在 flow 上设置非 2xx 响应将返回该响应并断开连接。CONNECT 不是常用的 HTTP 请求方法，目的是与服务器建立代理连接，仅是 client 与 proxy 的之间的交流，所以 CONNECT 请求不会触发 request、response 等其他常规的 HTTP 事件。

def requestheaders(self, flow: mitmproxy.http.HTTPFlow):
(Called when) 来自客户端的 HTTP 请求的头部被成功读取。此时 flow 中的 request 的 body 是空的。

def request(self, flow: mitmproxy.http.HTTPFlow):
(Called when) 来自客户端的 HTTP 请求被成功完整读取。

def responseheaders(self, flow: mitmproxy.http.HTTPFlow):
(Called when) 来自服务端的 HTTP 响应的头部被成功读取。此时 flow 中的 response 的 body 是空的。

def response(self, flow: mitmproxy.http.HTTPFlow):
(Called when) 来自服务端端的 HTTP 响应被成功完整读取。

def error(self, flow: mitmproxy.http.HTTPFlow):
(Called when) 发生了一个 HTTP 错误。比如无效的服务端响应、连接断开等。注意与“有效的 HTTP 错误返回”不是一回事，后者是一个正确的服务端响应，只是 HTTP code 表示错误而已。

（好了，你可以跳到示例了。）

2. 针对 TCP 生命周期
def tcp_start(self, flow: mitmproxy.tcp.TCPFlow):
(Called when) 建立了一个 TCP 连接。

def tcp_message(self, flow: mitmproxy.tcp.TCPFlow):
(Called when) TCP 连接收到了一条消息，最近一条消息存于 flow.messages[-1]。消息是可修改的。

def tcp_error(self, flow: mitmproxy.tcp.TCPFlow):
(Called when) 发生了 TCP 错误。

def tcp_end(self, flow: mitmproxy.tcp.TCPFlow):
(Called when) TCP 连接关闭。

3. 针对 Websocket 生命周期
def websocket_handshake(self, flow: mitmproxy.http.HTTPFlow):
(Called when) 客户端试图建立一个 websocket 连接。可以通过控制 HTTP 头部中针对 websocket 的条目来改变握手行为。flow 的 request 属性保证是非空的的。

def websocket_start(self, flow: mitmproxy.websocket.WebSocketFlow):
(Called when) 建立了一个 websocket 连接。

def websocket_message(self, flow: mitmproxy.websocket.WebSocketFlow):
(Called when) 收到一条来自客户端或服务端的 websocket 消息。最近一条消息存于 flow.messages[-1]。消息是可修改的。目前有两种消息类型，对应 BINARY 类型的 frame 或 TEXT 类型的 frame。

def websocket_error(self, flow: mitmproxy.websocket.WebSocketFlow):
(Called when) 发生了 websocket 错误。

def websocket_end(self, flow: mitmproxy.websocket.WebSocketFlow):
(Called when) websocket 连接关闭。

4. 针对网络连接生命周期
def clientconnect(self, layer: mitmproxy.proxy.protocol.Layer):
(Called when) 客户端连接到了 mitmproxy。注意一条连接可能对应多个 HTTP 请求。

def clientdisconnect(self, layer: mitmproxy.proxy.protocol.Layer):
(Called when) 客户端断开了和 mitmproxy 的连接。

def serverconnect(self, conn: mitmproxy.connections.ServerConnection):
(Called when) mitmproxy 连接到了服务端。注意一条连接可能对应多个 HTTP 请求。

def serverdisconnect(self, conn: mitmproxy.connections.ServerConnection):
(Called when) mitmproxy 断开了和服务端的连接。

def next_layer(self, layer: mitmproxy.proxy.protocol.Layer):
(Called when) 网络 layer 发生切换。你可以通过返回一个新的 layer 对象来改变将被使用的 layer。详见 layer 的定义。

5. 通用生命周期
def configure(self, updated: typing.Set[str]):
(Called when) 配置发生变化。updated 参数是一个类似集合的对象，包含了所有变化了的选项。在 mitmproxy 启动时，该事件也会触发，且 updated 包含所有选项。

def done(self):
(Called when) addon 关闭或被移除，又或者 mitmproxy 本身关闭。由于会先等事件循环终止后再触发该事件，所以这是一个 addon 可以看见的最后一个事件。由于此时 log 也已经关闭，所以此时调用 log 函数没有任何输出。

def load(self, entry: mitmproxy.addonmanager.Loader):
(Called when) addon 第一次加载时。entry 参数是一个 Loader 对象，包含有添加选项、命令的方法。这里是 addon 配置它自己的地方。

def log(self, entry: mitmproxy.log.LogEntry):
(Called when) 通过 mitmproxy.ctx.log 产生了一条新日志。小心不要在这个事件内打日志，否则会造成死循环。

def running(self):
(Called when) mitmproxy 完全启动并开始运行。此时，mitmproxy 已经绑定了端口，所有的 addon 都被加载了。

def update(self, flows: typing.Sequence[mitmproxy.flow.Flow]):
(Called when) 一个或多个 flow 对象被修改了，通常是来自一个不同的 addon。

示例
估计看了那么多的事件你已经晕了，正常，鬼才会记得那么多事件。事实上考虑到 mitmproxy 的实际使用场景，大多数情况下我们只会用到针对 HTTP 生命周期的几个事件。再精简一点，甚至只需要用到 http_connect、request、response 三个事件就能完成大多数需求了。

这里以一个稍微有点黑色幽默的例子，覆盖这三个事件，展示如果利用 mitmproxy 工作。

需求是这样的：

因为百度搜索是不靠谱的，所有当客户端发起百度搜索时，记录下用户的搜索词，再修改请求，将搜索词改为“360 搜索”；
因为 360 搜索还是不靠谱的，所有当客户端访问 360 搜索时，将页面中所有“搜索”字样改为“请使用谷歌”。
因为谷歌是个不存在的网站，所有就不要浪费时间去尝试连接服务端了，所有当发现客户端试图访问谷歌时，直接断开连接。
将上述功能组装成名为 Joker 的 addon，并保留之前展示名为 Counter 的 addon，都加载进 mitmproxy。
第一个需求需要篡改客户端请求，所以实现一个 request 事件：

def request(self, flow: mitmproxy.http.HTTPFlow):
    # 忽略非百度搜索地址
    if flow.request.host != "www.baidu.com" or not flow.request.path.startswith("/s"):
        return
 
    # 确认请求参数中有搜索词
    if "wd" not in flow.request.query.keys():
        ctx.log.warn("can not get search word from %s" % flow.request.pretty_url)
        return
 
    # 输出原始的搜索词
    ctx.log.info("catch search word: %s" % flow.request.query.get("wd"))
    # 替换搜索词为“360搜索”
    flow.request.query.set_all("wd", ["360搜索"])
第二个需求需要篡改服务端响应，所以实现一个 response 事件：

def response(self, flow: mitmproxy.http.HTTPFlow):
    # 忽略非 360 搜索地址
    if flow.request.host != "www.so.com":
        return
 
    # 将响应中所有“搜索”替换为“请使用谷歌”
    text = flow.response.get_text()
    text = text.replace("搜索", "请使用谷歌")
    flow.response.set_text(text)
第三个需求需要拒绝客户端请求，所以实现一个 http_connect 事件：

def http_connect(self, flow: mitmproxy.http.HTTPFlow):
    # 确认客户端是想访问 www.google.com
    if flow.request.host == "www.google.com":
        # 返回一个非 2xx 响应断开连接
        flow.response = http.HTTPResponse.make(404)
为了实现第四个需求，我们需要将代码整理一下，即易于管理也易于查看。

创建一个 joker.py 文件，内容为：

import mitmproxy.http
from mitmproxy import ctx, http
 
 
class Joker:
    def request(self, flow: mitmproxy.http.HTTPFlow):
        if flow.request.host != "www.baidu.com" or not flow.request.path.startswith("/s"):
            return
 
        if "wd" not in flow.request.query.keys():
            ctx.log.warn("can not get search word from %s" % flow.request.pretty_url)
            return
 
        ctx.log.info("catch search word: %s" % flow.request.query.get("wd"))
        flow.request.query.set_all("wd", ["360搜索"])
 
    def response(self, flow: mitmproxy.http.HTTPFlow):
        if flow.request.host != "www.so.com":
            return
 
        text = flow.response.get_text()
        text = text.replace("搜索", "请使用谷歌")
        flow.response.set_text(text)
 
    def http_connect(self, flow: mitmproxy.http.HTTPFlow):
        if flow.request.host == "www.google.com":
            flow.response = http.HTTPResponse.make(404)
 
创建一个 counter.py 文件，内容为：

import mitmproxy.http
from mitmproxy import ctx
 
 
class Counter:
    def __init__(self):
        self.num = 0
 
    def request(self, flow: mitmproxy.http.HTTPFlow):
        self.num = self.num + 1
        ctx.log.info("We've seen %d flows" % self.num)
 
创建一个 addons.py 文件，内容为：

import counter
import joker
 
addons = [
    counter.Counter(),
    joker.Joker(),
]
 
将三个文件放在相同的文件夹，在该文件夹内启动命令行，运行：

mitmweb -s addons.py
老规矩，关闭所有 Chrome 窗口，从命令行中启动 Chrome 并指定代理且忽略证书错误。

测试一下运行效果：







 最后
以上便是全部内容。Have fun and good luck！

参考：

mitmproxy 官方文档：https://docs.mitmproxy.org/stable/
mitmproxy 脚本示例：https://github.com/mitmproxy/mitmproxy/tree/master/examples
维基百科 - 代理服务器：https://zh.wikipedia.org/wiki/代理服务器