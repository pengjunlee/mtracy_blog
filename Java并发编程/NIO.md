> 原文链接：<https://msd.misuland.com/pd/3255818135034404610>

# 前言
上文《从入门到放弃-SpringBoot》SpringBoot源码分析-请求过程中我们了解到，tomcat接收、返回请求的过程都是基于NIO实现的。日常工作中有很多基于NIO的使用，我们知道NIO可以提高系统的并发度，接下来的系列我们来深入学习下NIO，本文先从使用上简单概述。

# NIO概述
`NIO`即`non-blocking`（New IO），是指`jdk1.4`及以上版本里提供的新api。

`NIO`和`IO`最大的区别：**IO是以流的方式处理数据**，而**NIO是以块的方式处理数据**；**IO对事件的处理是阻塞的**，**NIO是非阻塞的**。

NIO的核心部分：

- Channel
- Buffer
- Selector

NIO主要分为标准输入输出和网络请求

# 标准输入输出NIO
## 读取

	private static void readNio() {
	    try {
	        //1、开启文件读取流
	        FileInputStream fileInputStream = new FileInputStream("/Users/my/Desktop/123.txt");
	
	        //2、获取fileChannel
	        FileChannel channel = fileInputStream.getChannel();
	
	        //3、设置ByteBuffer大小，一次能容纳capacity字节
	        int capacity = 9;
	        ByteBuffer bf = ByteBuffer.allocate(capacity);
	
	        //4、当read返回-1时，表示文件读取完毕
	        int length = -1;
	        while ((length = channel.read(bf)) != -1) {
	
	            byte[] bytes = bf.array();
	            System.out.println(new String(bytes, 0, length));
	
	            //4、将bf position置为0，方便下次读取
	            bf.clear();
	
	        }
	        channel.close();
	        fileInputStream.close();
	    } catch (IOException e) {
	        e.printStackTrace();
	    }
	}

## 写入

	private static void writeNio() {
	    try {
	        //1、打开文件写入流
	        FileOutputStream fileOutputStream = new FileOutputStream("/Users/my/Desktop/123.txt");
	
	        //2、获取fileChannel
	        FileChannel channel = fileOutputStream.getChannel();
	
	        //3、初始化byteBuffer
	        String str = "萨达案发生大大sdada34;sdds'";
	        ByteBuffer bf = ByteBuffer.allocate(1024);
	
	        //4、将bf position置为0，方便下次读取
	        bf.clear();
	
	
	        //5、从byteBuffer的position位置填充byte
	        bf.put(str.getBytes());
	
	        //6、将bf position置为0，limit设置为position避免写入内容过多
	        bf.flip();
	
	        int length = 0;
	
	        //7、如果position小于limit即未写入完毕
	        while (bf.hasRemaining()) {
	            //8、将buffer内容写入channel
	            length = channel.write(bf);
	            System.out.println(bf);
	        }
	        channel.close();
	        fileOutputStream.close();
	    } catch (IOException e) {
	        e.printStackTrace();
	    }
	}

# 网络NIO
## 服务端
	package com.my.tools.nio;
	
	import java.io.IOException;
	import java.net.InetSocketAddress;
	import java.nio.ByteBuffer;
	import java.nio.channels.SelectionKey;
	import java.nio.channels.Selector;
	import java.nio.channels.ServerSocketChannel;
	import java.nio.channels.SocketChannel;
	import java.util.Iterator;
	
	public class ServerSocket {
	    private static ServerSocket serverSocket;
	
	    private Selector selector;
	
	    public static void main(String[] args) throws Exception {
	        ServerSocket.getInstance().init(8001).listen();
	    }
	
	    public static ServerSocket getInstance() {
	        if (serverSocket == null) {
	            synchronized (ServerSocket.class) {
	                if (serverSocket == null) {
	                    serverSocket = new ServerSocket();
	                }
	            }
	        }
	        return serverSocket;
	    }
	
	    public ServerSocket init(int port) throws IOException {
	        //初始化channel
	        ServerSocketChannel server = ServerSocketChannel.open();
	
	        //绑定本机8001端口
	        server.socket().bind(new InetSocketAddress(8001));
	
	        //设置为非阻塞模式
	        server.configureBlocking(false);
	
	        //开启selector管理器
	        selector = Selector.open();
	
	        //将selector注册至server，并设置只处理accept事件
	        server.register(selector, SelectionKey.OP_ACCEPT);
	
	        return this;
	    }
	
	    public void listen() throws Exception {
	        System.out.println("server start");
	
	        //无限循环持续监听
	        while (true) {
	            //会阻塞 直到监听到注册的事件
	            selector.select();
	
	            //获取唤醒的事件
	            Iterator<SelectionKey> selectorKeys = selector.selectedKeys().iterator();
	
	            while (selectorKeys.hasNext()) {
	                SelectionKey key = selectorKeys.next();
	
	                //将已取出的SelectionKey删除，防止重复处理
	                selectorKeys.remove();
	
	                if (key.isAcceptable()) {
	
	                    //获取到服务端的socket
	                    ServerSocketChannel serverSocketChannel = (ServerSocketChannel) key.channel();
	
	                    //获取接收到的客户端socket
	                    SocketChannel socketChannel = serverSocketChannel.accept();
	                    socketChannel.configureBlocking(false);
	
	                    //向客户端写消息
	                    socketChannel.write(ByteBuffer.wrap(new String("hello, this is server").getBytes()));
	
	                    //注册监听read事件
	                    socketChannel.register(selector, SelectionKey.OP_READ);
	                    System.out.println("accept");
	                } else if (key.isReadable()) {
	                    //使用selector获取channel
	                    SocketChannel socketChannel = (SocketChannel) key.channel();
	                    socketChannel.configureBlocking(false);
	
	                    ByteBuffer buffer = ByteBuffer.allocate(1024);
	
	                    //读消息
	                    int length = socketChannel.read(buffer);
	
	                    String string = new String(buffer.array(), 0 , length);
	
	                    System.out.println("read:" + socketChannel + string);
	
	                    //写消息
	                    socketChannel.write(ByteBuffer.wrap(("server " + System.currentTimeMillis()).getBytes()));
	                    Thread.sleep(10000);
	                }
	            }
	        }
	    }
	
	}

## 客户端

	package com.my.tools.nio;
	
	import java.io.IOException;
	import java.net.InetSocketAddress;
	import java.nio.ByteBuffer;
	import java.nio.channels.SelectionKey;
	import java.nio.channels.Selector;
	import java.nio.channels.SocketChannel;
	import java.util.Iterator;
	
	public class ClientSocket {
	    public static ClientSocket clientSocket;
	
	    private static Selector selector;
	
	    public static void main(String[] args) throws Exception {
	        ClientSocket.getInstance().init("localhost", 8001).listen();
	    }
	
	    public static ClientSocket getInstance() {
	        if (clientSocket == null) {
	            synchronized (ClientSocket.class) {
	                if (clientSocket == null) {
	                    clientSocket = new ClientSocket();
	                }
	            }
	        }
	
	        return clientSocket;
	    }
	
	    public ClientSocket init(String ip, int port) throws IOException {
	        SocketChannel socketChannel = SocketChannel.open();
	        socketChannel.connect(new InetSocketAddress(ip, port));
	        socketChannel.configureBlocking(false);
	
	        selector = Selector.open();
	        socketChannel.register(selector, SelectionKey.OP_CONNECT | SelectionKey.OP_READ);
	
	        return this;
	    }
	
	    public void listen() throws Exception {
	        System.out.println("client start");
	
	        while (true) {
	            selector.select();
	
	            Iterator<SelectionKey> selectionKeys = selector.selectedKeys().iterator();
	
	            while (selectionKeys.hasNext()) {
	                SelectionKey selectionKey = selectionKeys.next();
	                selectionKeys.remove();
	
	                if (selectionKey.isConnectable()) {
	                    SocketChannel socketChannel = (SocketChannel) selectionKey.channel();
	                    socketChannel.configureBlocking(false);
	
	                    ByteBuffer buffer = ByteBuffer.wrap(new String("hello, this is client").getBytes());
	                    socketChannel.write(buffer);
	
	                    socketChannel.register(selector, SelectionKey.OP_READ);
	                    System.out.println("client write");
	                } else if (selectionKey.isReadable()) {
	                    SocketChannel socketChannel = (SocketChannel) selectionKey.channel();
	                    socketChannel.configureBlocking(false);
	
	                    ByteBuffer buffer = ByteBuffer.allocate(1024);
	
	                    int length = socketChannel.read(buffer);
	
	                    System.out.println("client read: " + socketChannel + new String(buffer.array(), 0, length));
	
	                    socketChannel.write(ByteBuffer.wrap(("client " + System.currentTimeMillis()).getBytes()));
	
	                    Thread.sleep(10000);
	                }
	            }
	
	        }
	    }
	}

# 总结
上述示例展示了最简单的文件`NIO`和网络`NIO`用法，接下来会深入分析每个方法的源码，并对性能进行调优。