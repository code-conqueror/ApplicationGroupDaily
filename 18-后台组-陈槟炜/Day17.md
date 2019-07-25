[toc]
# IO流
## 字符流
### 字符流的缓冲区
- 缓冲区的出现提高了对数据的读写效率
- **对应类**
    - BufferedWriter
    - BufferedReader
- 缓冲区要结合流才可以使用
- 在流的基础上对流的功能进行增强

#### BufferedWriter(字符写入缓冲区)
##### 一、步骤 
1. 创建一个字符写入流对象
2. 为了提高字符写入流效率，加入了缓冲技术
3. 刷新
4. 关闭资源
##### 二、示例
```
package test;

import java.io.*;

public class BufferedWriterDemo {

	public static void main(String[] args) throws IOException
	{
		//创建一个字符写入流对象
		FileWriter fw = new FileWriter("buf.txt");
		//为了提高字符写入流效率，加入了缓冲技术
		//只要将需要被提高效率的流对象作为参数传递给缓冲区的构造函数即可
		BufferedWriter bufw = new BufferedWriter(fw);
		bufw.write("abcde");	
		//newLine()	-->换行，可跨平台
		bufw.newLine();   
		//只要用到缓冲区，就要记得刷新
		bufw.flush();
		//关闭缓冲区就是在关闭缓冲区中的流对象
		bufw.close();
	}

}

```
#### BufferedReader（字符写出缓冲区）
##### 一、步骤
1. 创建一个字符读取流对象与文件相关联
2. 为了提高效率，加入了缓冲技术，将字符读取流对象作为参数传递给缓冲区的构造函数
3. 调用readLine() 一行一行获取文本数据，当返回null,表示读到文件结尾
4. 关闭资源

```
注意: 
1.readLine方法返回的时候只返回回车符之前的数据内容，并不返回回车符。
2.readLine方法的原理：read方法一个读一个
```

##### 二、示例
```
package test;

import java.io.*;

public class BufferedReaderDemo {
	public static void main(String[] args) throws IOException
	{
		//创建一个字符读取流对象与文件相关联
		FileReader fr = new FileReader("buf.txt");
		//为了提高效率，加入了缓冲技术，将字符读取流对象作为参数传递给缓冲区的构造函数
		BufferedReader bufr = new BufferedReader(fr);
		String line = null;
		while((line=bufr.readLine())!=null)
		{
			System.out.println(line);
		}
		bufr.close();
	}
}
运行结果
abcde
```
#### 通过缓冲区复制文本文件
示例
```
package test;
import java.io.*;
public class CopyTextByBuf {
	public static void main(String[] args)
	{
		BufferedWriter bufw = null;
		BufferedReader bufr = null;
		try
		{
			bufr = new BufferedReader(new FileReader("src\\test\\BufferedWriterDemo.java"));
			bufw = new BufferedWriter(new FileWriter("Copy.txt"));
			String line = null;
			while((line=bufr.readLine())!=null)
			{
				bufw.write(line);
				bufw.newLine();
				bufw.flush();
			}
		}
		catch(IOException e)
		{
			throw new RuntimeException("读写失败");
		}
		finally
		{
			try
			{
				if(bufr!=null)
					bufr.close();
			}
			catch(IOException e)
			{
				throw new RuntimeException("读取关闭失败");
			}
			try
			{
				if(bufw!=null)
					bufw.close();
			}
			catch(IOException e)
			{
				throw new RuntimeException("写入关闭失败");			
			}
		}
	}
}
```

#### 装饰设计模式
1. 想要对已有的对象进行功能增强时，可以定义类，将以有对象传入，基于已有功能，并提供加强功能，自定义的该类称为装饰类
2. 装饰类通常会通过构造函数方法接受被装饰的对象，并给予被装饰类的对象的功能，提供更强的功能
- **示例**
```
package test;
/**
 * 装饰设计模式的演示
 * @author code-conqueror
 *
 */
public class demo {
	public static void main(String[] args)
	{
		Person p = new Person();
		Student s = new Student(p);
		s.superChifan();
	}
}
class Person
{
	public void Chifan()
	{
		System.out.println("吃饭");
	}
}
class Student
{
	private Person p;
	Student(Person p)
	{
		this.p = p;
	}
	public void superChifan()
	{
		System.out.println("吃菜");
		p.Chifan();
		System.out.println("喝汤");
	}
}
运行结果
吃菜
吃饭
喝汤

```

##### 装饰和继承的区别
1. 装饰模式比继承更灵活，避免了继承体系臃肿
2. 降低了类和类的关系。
3. 装饰类因为增强了已有对象，具备的功能和已有的都是相同的，只不过提供了更强的功能。所以装饰类和被装饰类通常都属于一个体系中。

#### LineNumberReader
- **setLineNumber(int)** 设置初始行数
- **getLineNumber()** 打印行数

## 字节流
**示例：**
```
package test;
import java.io.*;
public class FileStream {
	public static void main(String[] args) throws IOException
	{
		readFile_2();
	}
	public static void readFile_3() throws IOException
	{
		FileInputStream fis = new FileInputStream("fos.txt");
		byte[] buf = new byte [fis.available()];
		fis.read(buf);
			System.out.println(new String(buf));
		fis.close();
	}
	public static void readFile_2() throws IOException
	{
		FileInputStream fis = new FileInputStream("fos.txt");
		byte[] buf = new byte[1024];
		int len=0;
		while ((len=fis.read(buf))!=-1)
			System.out.println(new String(buf,0,len));
		fis.close();
	}
	public static void readFile_1() throws IOException
	{
		FileInputStream fis = new FileInputStream("fos.txt");
		int ch = 0;
		while ((ch=fis.read())!=-1)
			System.out.println((char)ch);
	}
	public static void writeFile()  throws IOException
	{
		//注意：在没用到缓冲区前不用刷新
		FileOutputStream fos = new FileOutputStream("fos.txt");
		fos.write("abcde".getBytes());
		fos.close();
	}
}

```

## 转换流
### 读取转换流
示例

```
package test;
import java.io.*;
public class TransStreamDemo {

	public static void main(String[] args) throws IOException
	{
		//获取键盘录入对象
		InputStream in =System.in;
		//将字节流对象转成字符流对象，使用转换流InputStreamReader
		InputStreamReader isr = new InputStreamReader(in);
		//为了提高效率，将字符串进行缓冲区技术高效操作
		BufferedReader bufr = new BufferedReader(isr);
		String line = null;
		while((line=bufr.readLine())!=null)
		{
			if("over".equals(line))
				break;
			System.out.println(line.toUpperCase());
		}
		bufr.close();
	}

}

```

### 写入转换流
示例
```
package test;
import java.io.*;
public class TransStreamDemo {

	public static void main(String[] args) throws IOException
	{
		//获取键盘录入对象
		//InputStream in =System.in;
		//将字节流对象转成字符流对象，使用转换流InputStreamReader
		//InputStreamReader isr = new InputStreamReader(in);
		//为了提高效率，将字符串进行缓冲区技术高效操作
		//BufferedReader bufr = new BufferedReader(isr);
		
		//键盘录入最常见写法
		BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
		
		//OutputStream out = System.out;
		//OutputStreamWriter osw = new OutputStreamWriter(out);
		//BufferedWriter bufw = new BufferedWriter(osw);
		
		BufferedWriter bufw = new BufferedWriter(new OutputStreamWriter(System.out));
		
		
		String line = null;
		while((line=bufr.readLine())!=null)
		{
			if("over".equals(line))
				break;
			bufw.write(line.toUpperCase());
			bufw.newLine();
			bufw.flush();
		}
		bufr.close();
	}

}

```

## File类
### File类概述
- 用来将文件或者文件夹封装成对象
- 方便对文件与文件夹的属性信息进行操作
- File对象可以作为参数传递给流的构造函数

### File类创建对象的方法
```
方法一：
		File f1 = new File("f:\\abc\\a.txt");
方法二：
		File f2 = new File("f:\\abc","b.txt");
方法三：
		File d = new File("f:\\abc");
		File f3 = new File(d,"c.txt");
注意：File.separator为目录分隔符，可跨平台使用
```
**示例：**
```
package test;
import java.io.*;
/**
 * file类对象的创建
 * @author code-conqueror
 *
 */
public class FileDemo {
	public static void main(String[] args) 
	{
		consMethod();

	}
	public static void consMethod()
	{
		//创建File对象的三种方法
		//方法一：
		//将a.txt封装成file对象。可以将已有的和未出现的文件或者文件夹封装成对象
		File f1 = new File("f:\\abc\\a.txt");
		//方法二：
		File f2 = new File("f:\\abc","b.txt");
		//方法三：
		File d = new File("f:\\abc");
		File f3 = new File(d,"c.txt");
		
		//目录分隔符 separator,可跨平台
		File f4 = new File("f:"+File.separator+"abc"+File.separator+"d.txt");
		System.out.println("f1: "+f1);  //f1: f:\abc\a.txt
		System.out.println("f2: "+f2);  //f2: f:\abc\b.txt
		System.out.println("f3: "+f3);  //f3: f:\abc\c.txt

		
	}

}
```

### File类常见方法
#### 一、创建
- **boolean createNewFile()**</br>
在指定位置上创建文件，返回true。如果文件已经存在，则不创建，返回false。
- **boolean mkdir()**</br>
创建文件夹
- **boolean mkdir()**</br>
创建多级文件夹

#### 二、删除
- **boolean delete()**</br>
    删除此抽象路径名表示的文件或目录。删除失败返回false。
- **void deleteOnExit()**</br>
在程序退出时删除指定文件

#### 三、判断
==在判断文件对象是否是文件或者目录时，必须先判断文件对象封装的内容是否存在==
- **boolean canExecute()**</br>
    判断这个文件是否可执行
- **boolean exists**</br>
    判断文件是否存在
- **boolean isFile()**</br>
    判断该文件对象是不是文件
- **boolean isDirectory()**</br>
    判断该文件对象是不是文件夹
- **boolean isHidden()**</br>
    判断指定的文件是否隐藏
- **boolean isAbsolute**</br>
    判断文件是不是抽象（绝对路径和相对路径）

#### 四、获取信息
- **getName()**</br>
    获取文件名
- **getPath()**</br>
    获取当前路径，绝对路径放回绝对路径，相对路径返回相对路径
- **getParent()**</br>
    返回上的时绝对路径下的父目录。</br>如果获取的相对路径，返回为null。</br>如果相对路径中有上一层目录那么该目录就时返回结果。
- **getAbsolutePath()**</br>
    获取绝对路径
- **long lastModified()**</br>
    返回文件最后一次被修改的时间
- **long length()**</br>
    返回文件的长度
- **A.renameTo(B)**</br>
    A的名字改为B

### 文件列表
- **File[] listRoots()**</br>
    返回系统盘符</br>
```
    File[] files = File.listRoots();
	for(File fl :files)
		System.out.println(fl);
```
- **String[] list()** </br>
返回指定目录下的文件和文件夹的名称。</br>
```
如：File f = new File("f:\\");
	String[] names = f.list();
	for(String name :names)
		System.out.println(name);
```
**注意：** 调用list方法的file对象必须是封装了一个目录。该目录还必须存在。
- **File[] listFiles()**</br>
返回一个抽象路径名数组，这些路径名表示此抽象路径名表示的目录中的文件。
```
如：File dir = new File("f:\\");
	File[] files = dir.listFiles();
	for(File f:files)
		System.out.println(f.getName()+"::"+f.length());
```
#### 示例（查找f盘下的图片）
```
package test;
import java.io.*;
/**
 * 查找指定目录下的图片文件
 * @author code-conqueror
 *
 */
public class FileDemo {
	public static void main(String[] args) throws IOException
	{
		File dir = new File("f:\\");
		String[] arr = dir.list(new FilenameFilter()
				{
					public boolean accept(File dir,String name)
					{
						return name.endsWith(".jpg");
					}
				});
		System.out.println("len:"+arr.length);
		for(String name : arr)
			System.out.println(name);
	}
}
运行结果：
len:1
timg.jpg
```
### 列出目录下所有内容
```
package test;
import java.io.*;
public class FileDemo3 {
	public static void main(String[] args) 
	{
		File dir = new File("f:\\");
		showDir(dir);
	}
	public static void showDir(File dir)
	{
		System.out.println(dir);
		File[] files = dir.listFiles();
		for(int x=0;x<files.length;x++)
		{
			if(files[x].isDirectory())
				showDir(files[x]);
			else
				System.out.println(files[x]);
		}
	}
}
```
### 删除带内容的目录
1. **删除原理：**
在window中，删除目录聪慧里面往外删除
2. **示例**
```
package test;
import java.io.*;
public class RemoveDir {
	public static void main(String[] args)
	{
		File dir = new File("E:\\3dm");
		removeDir(dir);
	}
	public static void removeDir(File dir)
	{
		File[] files = dir.listFiles();
		for(int x=0;x<files.length;x++)
		{
			if(files[x].isDirectory())
				removeDir(files[x]);
			else
				System.out.println(files[x].toString()+"::"+files[x].delete());
		}
		System.out.println(dir+"::"+dir.delete());
	}
}

```
## Properties
### Properties简述
Properties是hashtable的子类。也就是它具备了map集合的特点，而且它里边存储的键值对都是字符串。
- Properties是集合和IO技术相结合的集合容器
- **Properties对象的特点：** 可以用于键值对形式的配置文件
- 在加载数据是，需要数据有固定格式：**键=值**

### Properties存取
**一、步骤**
1. 创建Properties类对象
2. 用该对象调用setProperties(key,value)输入数据
3. 调用stringPropertyNames()方法获得键集
4. 调用getProperty(key)获得键值</br>

**二、示例：**
```
package test;

import java.io.*;
import java.util.*;
public class PropertiesDemo {
	public static void main(String[] args) 
	{
		setAndGet();
	}
	//设置和获取元素
	public static void setAndGet()
	{
		Properties prop = new Properties();
		prop.setProperty("zhangsan", "30");
		prop.setProperty("lisi", "39");
		//System.out.println(prop);
		String value = prop.getProperty("lisi");
		System.out.println(value);
		Set<String> names = prop.stringPropertyNames();
		for(String s:names)
			System.out.println(s+"="+prop.getProperty(s));
	}
}
运行结果
39
lisi=39
zhangsan=30

```
### Properties存取配置文件
**示例：**
```
package test;
import java.io.*;
import java.util.*;
public class PropertiesDemo1 {
	public static void main(String[] args) throws IOException 
	{
		//method_1();
		method_3();
	}
	public static void method_3() throws IOException
	{
		FileInputStream fis = new FileInputStream("info.txt");
		Properties prop =new Properties();
		//将流中的数据加载进集合
		prop.load(fis);
		prop.setProperty("wangwu","41");	
		FileOutputStream fos = new FileOutputStream("info.txt");
		prop.store(fos, "hashdk");		//用于注释	
		prop.list(System.out);
		fos.close();
		fis.close();
		/*
		 * -- listing properties --
				lisi=40
				zhangsan=39
				wangwu=41

		 */

	}
	public static void method_2() throws IOException
	{
		FileInputStream fis = new FileInputStream("info.txt");
		Properties prop =new Properties();
		//将流中的数据加载进集合
		prop.load(fis);
		//System.out.println(prop);	//{lisi=40, zhangsan=39, wangwu=35}
		prop.setProperty("wangwu","41");	
		prop.list(System.out);	
		/*-- listing properties --
		lisi=40
		zhangsan=39
		wangwu=41
		info.txt中wangwu=35，没有改变
		*/
		
	}
	public static void method_1() throws IOException 
	{
		Properties prop = new Properties(); 
		BufferedReader bufr = new BufferedReader(new FileReader("info.txt"));
		String line = null;
		while((line = bufr.readLine())!=null)
		{
			String[] arr = line.split("=");
			prop.setProperty(arr[0], arr[1]);
			
		}
		bufr.close();
		System.out.println(prop);	//{lisi=40, zhangsan=39, wangwu=35}
	}
}

```
### Properties练习
## 打印流
- PrintWriter与PrintStream</br>
    **可以直接操作输入流和文件,可以将各种数据类型的数据都原样打印**

### PrintStream
构造函数可以接收的类型：
1. file对象。File
2. 字符串路径。String
3. 字节输出流。OutputStream

### PrintWriter（字节打印流）
构造函数可以接收的类型：
1. file对象。File
2. 字符串路径。String
3. 字节输出流。OutputStream
4. 字符输出流。Writer
**示例**</br>
```
package test;
import java.io.*;
public class PrintStreamDemo {
	public static void main(String[] args) throws IOException
	{
		BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
		PrintWriter out = new PrintWriter(System.out);
		String line = null;
		while((line=bufr.readLine())!=null)
		{
			if("over".equals(line))
				break;
			out.println(line.toUpperCase());
			out.flush();
		}
		out.close();
		bufr.close();
		
	}
}

```

## 序列流
- SequenceInputStream</br>
    **对多个文件进行合并**
- 示例：
```
package test;
import java.io.*;
import java.util.*;
public class SequenceDemo {

	public static void main(String[] args) throws IOException 
	{
		Vector<FileInputStream> v =new Vector<FileInputStream>();
		v.add(new FileInputStream("f:\\1.txt"));		
		v.add(new FileInputStream("f:\\2.txt"));	
		v.add(new FileInputStream("f:\\3.txt"));
		
		Enumeration<FileInputStream> en = v.elements();
		SequenceInputStream sis = new SequenceInputStream(en);
		FileOutputStream fos = new FileOutputStream("f:\\4.txt");
		byte[] buf = new byte[1024];
		int len = 0;
		while((len=sis.read(buf))!=-1)
			fos.write(buf,0,len);
		fos.close();
		sis.close();
	}

}

```
## 切割文件
示例：
```
package test;
import java.util.*;
import java.io.*;
public class SplitFile {
	public static void main(String[] args) throws IOException
	{
		//splitFile();
		merge();
	}
	public static void merge() throws IOException
	{
		ArrayList<FileInputStream> al= new ArrayList<FileInputStream>();
		for(int x=1;x<=3;x++)
		{
			al.add(new FileInputStream("f:\\splitfiles\\"+x+".part"));
		}
		final Iterator<FileInputStream> it = al.iterator();
		Enumeration<FileInputStream> en = new Enumeration<FileInputStream>()
		{
			public boolean hasMoreElements()
			{
				return it.hasNext(); 
			}
			public FileInputStream nextElement()
			{
				return it.next();
			}
		};
		SequenceInputStream sis = new SequenceInputStream(en);
	}
	public static void splitFile() throws IOException
	{
		FileInputStream fis = new FileInputStream("F:\\1.bmp");
		FileOutputStream fos = null;
		byte[] buf = new byte[1024*1024];
		int len = 0;
		int count = 1;
		while((len=fis.read(buf))!=-1)
		{
			fos = new FileOutputStream("f:\\spltfiles\\"+(count++)+".part");
			fos.write(buf,0,len);
			fos.close();
		}
	}
	
}

```
## 操作对象
- ObjectInputStream与ObjectOutputStream</br>
    **被操作对象需要实现Serializable(标记接口)**

    
