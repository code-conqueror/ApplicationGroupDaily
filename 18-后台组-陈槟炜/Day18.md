# IO流
[toc]
## 对象的序列化
- **序列化：** 就是把对象转化成字节。
- 没方法的接口就叫标记接口，如Serializable接口
- 实现Serializable接口就能实现序列化
- 新的类如何操纵曾经序列化的对象--**UID固定**</br>
`任意修饰符 static final long serialVersionUID = x`
- 静态成员是不能序列化，对象是在堆中，静态是在方法区中
- 非静态成员不想序列化的话，就加入transient修饰即可，保证其值在堆内存存在，而不在文本文件中存在
### 示例(对象序列化的实现）
```
package test;
import java.io.*;
public class ObjectStreamDemo 
{
	public static void main(String[] args) throws Exception
	{
		//writeObj();
		readObj();
	}
	public static void readObj() throws Exception
	{
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("obj.txt"));
		Person p = (Person)ois.readObject();
		System.out.println(p);
		ois.close();
	}
	public static void writeObj() throws IOException
	{
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("obj.txt"));
		oos.writeObject(new Person("lisi0",19, "kr"));
		oos.close();
	}
}
----------------
package test;
import java.io.*;
public class Person implements Serializable //Seriablizable接口实现序列化
{
	public static final long serialVersionUID = 42l;
	private String name;
	transient int age;	//保证其值只能在堆内存中存在
	static String country="cn";	//静态成员无法实现序列化
	Person(String name,int age,String country)
	{
		this.name = name;
		this.age = age;
		this.country=country;
	}
	public String toString()
	{
		return name+":"+age+":"+country;
	}
}

```

## 管道流
### 一、功能：
输入输出可以直接进行连接，通过结合线程使用
### 二、类：
PipedInputStream和PipedOutputStream
### 三、示例
```
package test;
import java.io.*;
class Read implements Runnable
{
	private PipedInputStream in;
	Read(PipedInputStream in)
	{
		this.in = in;
	}
	public void run()
	{
		try
		{
			byte[] buf = new byte[1024];
			int len = in.read(buf);
			String s = new String(buf,0,len);
			System.out.println(s);
			in.close();
		}
		catch(IOException e)
		{
			throw new RuntimeException("管道读取流失败");
		}
	}
}
class Write implements Runnable
{
	private PipedOutputStream out;
	Write(PipedOutputStream out)
	{
		this.out = out;
	}
	public void run()
	{
		try
		{
			out.write("piped lai".getBytes());
			out.close();
		}
		catch(IOException e)
		{
			throw new RuntimeException("管道输出流失败");
		}
	}
}
public class PipedStreamDemo 
{
	public static void main(String[] args) throws IOException
	{
		PipedInputStream in = new PipedInputStream();
		PipedOutputStream out = new PipedOutputStream();
		in.connect(out);
		Read r = new Read(in);
		Write w = new Write(out);
		new Thread(r).start();
		new Thread(w).start();
	}
	
}
运行结果：
piped lai

```

## RandomAccessFile
### 一、概述
- **功能：** 随机访问文件
- 直接继承自Object
- 是IO包的成员，具备了读和写的功能
- 内部封装了一个数组，通过指针堆数组中的元素进行操作。
- getFilePointer可以获取指针位置，通过seek改变指针的位置
- **完成读写的原理：** 内部封装了字节输入流和输出流
- **局限性：** 只能操作文件，而且操作文件有模式
    - "rw" 　　读和写
    - "r"　　　读
- 如果模式为 r,不会创建文件，会去读取一个已存在文件，如果该文件不存在，则会出现异常
- 如果模式为 rw。操作的文件不存在，会自动创建，如果存在则不会覆盖

### 二、常用方法
- **RandomAccessFile raf = new RandomAccessFile(路径,操作模式);**
    创建对象
- **write("王五".getByte())**   写入"王五"到文件中   
- **writeInt(256):** 写入256到文件中
- **seek(8);**  调整对象的指针,指向8
- **skipsdBytes(8);**   跳过指定的字节数，只能往后跳8  

## DataStream
### 一、功能：
操作基本数据类型
### 二、示例：

```
package test;
import java.io.*;
public class DataStreamDemo
{
	public static void main(String[] args) throws IOException
	{
		//writeData();
		//readData();
//		writeUTFDemo();
//		OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("utf.txt"),"utf-8");
//		osw.write("你好");
//		osw.close();
		readUTFDemo();
	}
	public static void writeUTFDemo() throws IOException
	{
		DataOutputStream dos = new DataOutputStream(new FileOutputStream("utfdate.txt"));
		dos.writeUTF("你好");	//写入"你好"到文件中
		dos.close();
	}
	public static void readUTFDemo() throws IOException
	{
		DataInputStream dis = new DataInputStream(new FileInputStream("utfdate.txt"));
		String s =dis.readUTF();
		System.out.println(s);
		dis.close();
	}
	public static void writeData() throws IOException
	{
		DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.txt"));
		dos.writeInt(234);
		dos.writeBoolean(true);
		dos.writeDouble(6451631.468);
		dos.close();
	}
	public static void readData() throws IOException
	{
		DataInputStream dis = new DataInputStream(new FileInputStream("data.txt"));
		//只能按顺序读取，否者会出现乱码
		int a = dis.readInt();
		boolean b = dis.readBoolean();
		double c = dis.readDouble();
		System.out.println(a);
		System.out.println(b);
		System.out.println(c);
		dis.close();
	}
}

```

## ByteArrayStream
### 一、功能
可以操作字节数组

### 二、类：
- **ByteArrayInputStream**</br>
    在构造时，需要接收数据源，而且数据源是一个字节数组
- **ByteArrayOutputStream**</br>
    在构造的时候，不用定义数据目的，因为该对象中已经封装了可变长度的字节数组，这就是数据目的地</br>
==因为这两个流对象都操作的数组，并没有使用系统资源，所以不用进行close关闭==

### 三、示例
```
流操作：
源设备：
	键盘 System.in	硬盘 FileStream	内存 ArrayStream
目的设备：
	控制台 System.out	硬盘 FileStream	内存 ArrayStream

```
```
package test;
import java.io.*;
public class ByteArrayStreamDemo {
	public static void main(String[] args)
	{
		//数据源
		ByteArrayInputStream bis = new ByteArrayInputStream("ABCDE".getBytes());
		//"ABCDE".getBytes() 将字符串转换为字符数组
		
		//数据目的
		ByteArrayOutputStream bos = new ByteArrayOutputStream();
		int by = 0;
		while((by=bis.read())!=-1)
		{
			bos.write(by);
		}
		System.out.println(bos.size());
		System.out.println(bos.toString());
	}
}
运行结果：
5
ABCDE
```
## 字符编码
### 常见编码表
- **ASCII：** 美国标准信息交换码
用一个字节的7位可以表示
- **Unicode：** 国际标准码，融合多种文字，所有文字都是有两个字节表示，java使用的就是Unicode
- **GBK：** 中国的中文编码表升级版，融合了更多的中文字符号，用四个字节
- **UTF-8：** 最多用三个字节表示一个字符
- **GB2312：** 中国的中文编码表
- **ISO8859-1：** 拉丁码表。欧洲码表

### 转化流的字符编码
**示例**
```
package test;
import java.io.*;
public class EncodeStream {
	public static void main(String[] args) throws IOException
	{
		//write();
		read();
	}
	public static void read() throws IOException
	{
		InputStreamReader isr = new InputStreamReader(new FileInputStream("utf.txt"),"GBK");
		char[] buf = new char[10];
		int len = isr.read(buf);
		String str = new String(buf,0,len);
		System.out.println(str);	//浣犲ソ
		isr.close();
	}
	public static void write() throws IOException
	{
		OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("utf.txt"),"UTF-8");
		osw.write("你好");
		osw.close();
	}
}
```

# GUI
