> 原文链接：<https://maimai.cn/article/detail?fid=1475769185&efid=hShXgLA0MbxFGcHTa88IFw&use_rn=1>

在 Linux 环境下工作的工程师，一定会对那些繁琐的指令和参数命令行印象深刻吧。而且，可怕的不是繁琐，而是需要大量重复输入这些繁琐的命令。

在 Linux 下我们有个别名命令 alias ，可以将那些繁琐的命令自定义为我们容易记住的别名，可以大大提高我们的效率。

但是，alias 命令只对当前终端有效，当终端关闭之后，我们所设置的别名全部失效。所以如果想让这些别名永久有效，我们就需要将它们添加到 .bash_profile 文件里。

在本文里，良许将介绍 10 个非常实用，能够提高你工作效率的命令别名。

1.
压缩包文件，特别是 tar 文件在 Linux 下使用非常广泛，但是 tar 命令的选项又非常多，也不好记住。所以我们可以将常用的几个选项定义为一个别名 untar ，这样我们需要解压 tar 文件时，直接 untar filename 即可。

alias untar='tar -zxvf '复制代码
2.
我们下载一个很大的文件时，突然网络异常中断了，我们重新下载是不是很抓狂？别担心，我们的 wget 命令有个 -c 选项，支持断点下载，我们也可以将它设置为别名：

alias wget='wget -c '复制代码
3.
有时我们需要生成一个 20 个字符的随机数密码，我们可以使用 openssl 命令，但完整的命令又很长很不方便，我们可以设置别名：

alias getpass="openssl rand -base64 20"复制代码
4.
下载一个文件之后，我们想要校验一下它的 checksum 值，可以将这个命令封装为一个别名 sha ，之后我们 sha filename 就可以校验文件的 checksum 值。

alias sha='shasum -a 256 '复制代码
5.
正常情况下，ping 命令将无限次输出，但其实没多大意义。我们可以使用 -c 命令将其限制为 5 次输出，然后设置为别名 ping ，使用时，ping url 即可。

alias ping='ping -c 5'复制代码
6.
如果我们想随时随地启动一个 web 服务器，我们可以使用这个别名：

alias www='python -m SimpleHTTPServer 8000'复制代码
7.
网速的测试在工作中也经常用到，但 Linux 没有自带命令可用，我们可以借助第三方工具 speedtest-cli 。这个工具可以直接从 Github 上下载，使用方法里面也有详细介绍。我们需要先使用 speedtest-cli 命令来选择离我们最近的服务器，然后设置如下别名：

alias speed='speedtest-cli --server 2406 --simple'复制代码
8.
你的公网 IP 是多少？记性好的可以直接背下来，但如果你有 10 台上百台服务器呢？也可以背下来，然后参加最强大脑。其实有个命令可以直接查询，但那个命令太变态，不好记，果断设置为别名。

alias ipe='curl ipinfo.io/ip'复制代码
9.
如何知道自己的局域网 IP ？这个命令同样变态，果断设置别名。

alias ipi='ipconfig getifaddr en0'复制代码
10.
最后，清屏，我们可以使用 ctrl + l 快捷键，也可以将 clear 命令定义得更短，这样使用起来更直接，更粗暴。

alias c='clear'复制代码
这 10 个命令你不一定完全都用得上，因为大家使用 Linux 的方向不一样，工作内容不一样。在你的工作领域也一定有大量复杂繁琐的命令可以定义为别名，欢迎在留言区补充！