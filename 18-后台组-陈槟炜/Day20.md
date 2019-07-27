[toc]
# 网络编程
## 概述
**一、数据传输方式**
1. 找到对方IP
2. 数据要发送到对方指定的应用程序上。</br>为了标识这些应用程序，所以给这些网络应用程序都采用数字进行标识。为了方便称呼数字就叫**端口**
3. 定义通讯规则。这个通讯规则为协议</br>
    国际组织定义通用协议TCP/IP
## 网络模型
**一、OSI参考模型**</br>
**数据封包**（层层封装）</br>
**数据拆包**

```
应用层                         应用层
表示层                          
会话层                          
传输层（tcp\udp)               传输层 
网络层（给数据IP地址）          网际层
数据链路层(通过什么方式出去)    主机至网络层
物理层（如：网线。光纤）
```
## 网络通讯要素
- IP地址
    - 网络中设备标识
    - 不易记忆，可用于主机名
    - 本地回环地址：127.0.0.1 主机名：localhost
- 端口号
    - 用于标识进程的逻辑地址，不同进程的标识
    - 有效端口：0~65535，其中0~1024系统使用或保留端口
- 传输协议
    - 通讯的规则
    - 参见协议：TCP、UDP

## IP地址
**一、常见方法**
1. 返回主机对象
```
IntAddress ia = IntAddress.fetLocalHost();     
```
2. 获得IP的主机地址
```
ia.getHostAddress();
```
3. 获得IP的主机名称
```
ia.getHostName();
```
4. 获得IP的主机名称和地址
```
ia.toString();
```
5. 获得任意一台主机的IP地址对象
IntAddress i = IntAddress.getByName("192.168.1.254");

## 传输协议
- UDP
    - 将数据及源和目的封装成数据包，不需要建立连接
    - 每个数据包的大小限制在64k内
    - 因无连接，是不可靠协议
    - 不需要建立连接，速度快
- TCP
    - 建立连接，形成传输数据的通道
    - 在连接中进行大数据量传输
    - 通过三次握手完成连接，是可靠协议
    - 必须建立连接，效率会稍低

## Socket
- Socket就是为网络服务提供一种机制
- 通讯两端都有Socket
- 网络通讯其实就是Socket间的通信
- 数据在两个Socket间通过IO传输

## Udp
### Udp-发送端
```
需求：通过Udp传输方式，将一段文字数据发送出去
```
**一、思路**
1. 建立udp服务
2. 确定数据，并封装成数据包.
3. 通过socket服务，将已有的数据包发送出去。
4. 关闭资源</br>

**二、示例**
```
package text3;
import  java.net.*;
/*
 * 通过udp传输方式，将一段文字数据发送出去
 * 思路：
 * 1.建立udp服务
 * 2.确定数据，并封装成数据包.
 * 3.通过socket服务，将已有的数据包发送出去。
 * 4.关闭资源
 */
public class Udp {
	public static void main(String[] args) throws Exception
	{
		//1.建立udp服务，通过DatagramSocket对象
		DatagramSocket ds = new DatagramSocket();
		//2.确定数据，并封装成数据包.
		byte[] buf = "udp ge men lai le".getBytes();
		DatagramPacket dp = new DatagramPacket(buf,buf.length,InetAddress.getByName("192.168.101.70"),10000);
		//3.通过socket服务，将已有的数据包发送出去。通过send方法
		ds.send(dp);
		//4.关闭资源
		ds.close();
	}

}

```
### Udp-接收端
```
需求：定义一个应用程序，用于接收Udp协议传输的数据并处理
```
**一、思路**
1. 定义udpscoket服务
2. 定义一个数据包因为要存储接收到的字节数据
3. 通过socket服务的receive方法将收到的数据存入定义好的数据包中
4. 通过数据包对象的特有功能，将这些不同的数据取出，并打印在控制台上
5. 关闭资源</br>

**二、示例**
```
package text3;
import java.net.*;
public class UdpRece {
	public static void main(String[] args) throws Exception
	{
		//1.建立Udp socket服务，建立端点
		DatagramSocket ds = new DatagramSocket(10000);
		
		//2. 定义一个数据包因为要存储接收到的字节数据
		byte[] buf = new byte[1024];
		
		DatagramPacket dp = new DatagramPacket(buf,buf.length);
		//3. 通过socket服务的receive方法将收到的数据存入定义好的数据包中
		ds.receive(dp);
		
		//4. 通过数据包对象的特有功能，将这些不同的数据取出，并打印在控制台上
		String ip = dp.getAddress().getHostAddress();
		String data = new String(dp.getData(),0,dp.getLength());
		int port = dp.getPort();	//获取端口
		System.out.println(ip+"::"+data+"::"+port);
		
		//5. 关闭资源
		ds.close();
		
	}
}

```

### Udp-聊天
```
package text3;
import java.net.*;
import java.io.*;
class Send implements Runnable
{
	private DatagramSocket ds;
	Send(DatagramSocket ds)
	{
		this.ds=ds;
	}
	public void run()
	{
		try
		{
			BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
			String line = null;
			while((line=bufr.readLine())!=null)
			{
				if("886".equals(line))
					break;
				byte[] buf = line.getBytes();
				DatagramPacket dp = new DatagramPacket(buf,buf.length,InetAddress.getByName("192.168.101.70"),10002);
				ds.send(dp);
			}
		}
		catch(Exception e)
		{
			throw new RuntimeException("发送端失败");
		}
	}
}
class Rece implements Runnable
{
	private DatagramSocket ds;
	Rece(DatagramSocket ds)
	{
		this.ds=ds;
	}
	public void run()
	{
		try
		{
			while(true)
			{
				byte[] buf = new byte[1024];
				DatagramPacket dp = new DatagramPacket(buf,buf.length);
				ds.receive(dp);
				String ip = dp.getAddress().getHostAddress();
				String data = new String(dp.getData(),0,dp.getLength());
				System.out.println(ip+":"+data);
			}
		}
		catch(Exception e)
		{
			throw new RuntimeException("接收端异常");
		}
	}
}
public class UdpDemo 
{
	public static void main(String[] args) throws Exception 
	{
		DatagramSocket sendSocket = new DatagramSocket();
		DatagramSocket receSocket = new DatagramSocket(10002);
		new Thread(new Send(sendSocket)).start();
		new Thread(new Rece(receSocket)).start();
	}

}

```

## TCP传输
### 概述
1. 分为客户端和服务端
2. 客户端的对象是socket，服务端的对象是SeverSocket

### 客户端
**一、概念**
1. 客户端的对象是socket对象。通过查阅socket对象，发现在该对象建立时，就可以去连接指定主机。</br>
```
因为tcp是面向连接的，所以在建立socket服务时，就要有服务端存在，并连接成功，形成通路后，在该通道进行数据的传输。`
```
**二、步骤**

1. 建立连接后，通过Sccket中的IO流进行数据的传输
2. 关闭socket

**示例**
```
package text3;
import java.io.*;
import java.net.*;

//客户端
public class TcpClient {
	public static void main(String[] args) throws Exception
	{
		//创建客户端的socket服务，指定目的主机和端口
		Socket s = new Socket("192.168.101.70",10003);
		
		//为了发送数据，应该获取socket流中输出流  ？
		OutputStream out = s.getOutputStream();
		out.write("tcp ge man l".getBytes());
		
		s.close();
	}
}

```

### 服务端
**一、步骤**
1. 建立服务端的socket服务。ServerSocket();
	并监听一个端口
2. 获取连接过来的客户端对象
	通过ServerSocker的 accept方法。没有连接就会等，所以这个方法是阻塞式的
3. 客户端如果发过来数据，那么服务端要使用对应的客户端对象，并获取到该客户端对象的读取流读取发过来的数据，并打印在控制台上
4. 关闭服务端（可选操作）</br>

**二、示例**
```
package text3;

import java.io.*;
import java.net.*;

/*
服务端
步骤：
1.建立服务端的socket服务。ServerSocket();
	并监听一个端口
2.获取连接过来的客户端对象
	通过ServerSocker的 accept方法。没有连接就会等，所以这个方法是阻塞式的
3.客户端如果发过来数据，那么服务端要使用对应的客户端对象，并获取到该客户端
	对象的读取流读取发过来的数据，并打印在控制台上
4. 关闭服务端（可选操作）
*/
public class TcpServer
{
	public static void main(String[] args) throws Exception
	{
		//建立服务端socket服务
		ServerSocket ss = new ServerSocket(10003);
		//获取客户端对象
		Socket s = ss.accept();
		
		//获取客户端发送过来的数据
		String ip = s.getInetAddress().getHostAddress();
		System.out.println(ip+"....connected");
		
		InputStream in = s.getInputStream();
		byte[] buf = new byte[1024];
		int len = in.read(buf);
		
		System.out.println(new String(buf,0,len));
		s.close();	//关闭客户端
		ss.close();
	}
}

```

## 示例（上传图片）
```
package tcp;
import java.io.*;
import java.net.*;
public class TcpServer 
{
	public static void main(String[] args) throws Exception
	{
		ServerSocket ss = new ServerSocket(10005);
		//获取客户端对象
		Socket s = ss.accept();
		//s.getInputStream(); -->返回此套接字的输入流。
		InputStream in = s.getInputStream();	//用处：读取Socket的数据
		//把数据写到一个文件上
		FileOutputStream fos = new FileOutputStream("server.jpg");
		byte[] buf = new byte[1024];
		int len = 0;
		//in.read(buf)-->返回读到字符数
		//如果因为流位于文件末尾而没有可用的字节，则返回值 -1
		while((len = in.read(buf))!=-1)		//用处？
		{
			//将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此输出流。
			//FilterOutputStream 的 write 方法依次调用带一个参数的 write 方法来输出每个 byte。
			fos.write(buf, 0, len);	//把读到的数据写入fos中
		}
			OutputStream out = s.getOutputStream();
			out.write("上传成功".getBytes());
			fos.close();
			s.close();
			ss.close();
		
	}
}
---------------------
package tcp;
import java.io.*;
import java.net.*;
public class TcpClient 
{
	public static void main(String[] args) throws Exception
	{
		Socket s = new Socket("192.168.101.70",10005);
		FileInputStream fis = new FileInputStream("F:\\timg.jpg");
		OutputStream out = s.getOutputStream();
		byte[] buf = new byte[1024];
		int len = 0;
		while((len=fis.read(buf))!=-1)
		{
			out.write(buf,0,len);
		}
		//告诉服务器已经写完
		s.shutdownOutput();
		InputStream in = s.getInputStream();
		byte[] bufIn = new byte[1024];
		//in.read(bufIn)-->从输入流中读取一定数量的字节，并将其存储在缓冲区数组 bufIn 中。以整数形式返回实际读取的字节数。
		int num = in.read(bufIn);
		//new String(bufIn,0,num)通过使用平台的默认字符集解码指定的 byte 子数组，构造一个新的 String。
		System.out.println(new String(bufIn,0,num));
		s.close();
		fis.close();
	}
}

```