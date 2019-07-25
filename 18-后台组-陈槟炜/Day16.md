[toc]
# 其他对象
## System
- 描述系统的一些信息
- 获取系统属性信息：getProperties();
```
package system;
import java.util.*;

public class SystemDemo {
	public static void main(String[] args)
	{
		Properties prop = System.getProperties();
		
		for(Object obj: prop.keySet())
		{
			String values = (String)prop.get(obj);
			System.out.println(obj+"::"+values);
		}
	}
}
```
## Runime
- 该类并没有提供构造函数</br>
说明不可以new对象，那么会直接想到该类中的方法都是静态的。</br>
发现该类中还有非静态方法。</br>
说明该类肯定会提供方法获取本类对象。而且该方法是静态的，并返回值类型是本类类型。</br>
- 由这个特点可以看出该类使用了单例设计模式完成。
```
package system; 
import java.io.IOException;
/**
 * 打开qq
 * @author code-conqueror
 *
 */
public class  RuntimeDemo{
	public static void main(String[] args) throws InterruptedException {
		try {
			Runtime r = Runtime.getRuntime();
			Process p = r.exec("F:\\Program Files (x86)\\Tencent\\QQ\\Bin\\QQScLauncher.exe");
			p.destroy();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

## Date类
- 示例：
```
package system;
import java.util.*;
import java.text.*;
/**
 * 改变打印时间的格式
 * @author DELL
 *
 */
public class DateDemo {
	public static void main(String[] args)
	{
		Date d = new Date();
		System.out.println(d);
		
		//将模式封装到SimpleDateformat对象中
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 hh:mm:ss");
		//调用format方法让模式格式化指定Date对象
		String time = sdf.format(d);
		System.out.println("time="+time);
	}
}

```
## Calendar类
- Calendar 类是一个抽象类，它为特定瞬间与一组诸如 YEAR、MONTH、DAY_OF_MONTH、HOUR 等 日历字段之间的转换提供了一些方法，并为操作日历字段（例如获得下星期的日期）提供了一些方法。
- 示例：
```
package system;
import java.util.*;
import java.text.*;
public class CalendarDemo {
	public static void main(String[] args)
	{
		//查表法
		String[] mon = {"一月","二月","三月","四月",
						"五月","六月","七月","八月",
						"九月","十月","十一月","十二月"};
		Calendar cl = Calendar.getInstance();
		System.out.println(cl);
		System.out.println(cl.get(Calendar.YEAR)+"年");
		//System.out.println((cl.get(Calendar.MONTH)+)+"月");
		int index =cl.get(Calendar.MONTH);
		System.out.println(mon[index]);
		System.out.println(cl.get(Calendar.DAY_OF_MONTH)+"日");
	}
}
运行结果
java.util.GregorianCalendar[time=1563872447691,areFieldsSet=true,areAllFieldsSet=true,lenient=true,zone=sun.util.calendar.ZoneInfo[id="Asia/Shanghai",offset=28800000,dstSavings=0,useDaylight=false,transitions=29,lastRule=null],firstDayOfWeek=1,minimalDaysInFirstWeek=1,ERA=1,YEAR=2019,MONTH=6,WEEK_OF_YEAR=30,WEEK_OF_MONTH=4,DAY_OF_MONTH=23,DAY_OF_YEAR=204,DAY_OF_WEEK=3,DAY_OF_WEEK_IN_MONTH=4,AM_PM=1,HOUR=5,HOUR_OF_DAY=17,MINUTE=0,SECOND=47,MILLISECOND=691,ZONE_OFFSET=28800000,DST_OFFSET=0]
2019年
七月
23日


```
## Random类
- Math.random(),生成[0,1)的随机数
- 用Random类创建对象，然后调用用nextInt(int n)，就可以产生[0,n)的随机整数
- 示例：
```
package text;
import java.util.*;
public class RandomDemo {
	public static void main(String[] args)
	{
		Random r = new Random();
		for(int i=0;i<100;i++)
		{
			if(i%10==0)	System.out.println();
			System.out.print(r.nextInt(10)+" ");
		}
		
	}
}
运行结果：

4 1 8 6 5 2 7 9 0 1 
9 9 6 1 0 4 8 7 0 1 
6 2 9 5 2 8 2 1 3 0 
8 7 6 3 0 6 1 2 1 5 
1 1 6 5 4 5 7 6 1 1 
3 2 5 4 8 9 2 1 1 4 
9 5 3 9 0 0 4 4 5 2 
8 5 7 3 8 8 1 1 7 4 
4 3 6 7 0 1 8 8 2 6 
8 7 8 4 9 4 2 4 8 0 
```

# IO流
## IO流概述
- IO流用来处理设备之间的数据传输
- Java对数据的操作是通过流的方式
- Java用于操作流的对象都在IO包中
- **流按操作数据分为两种：** 字节流和字符流
- **流按流向分为：** 输入流和输出流
## IO流常用基类
- **字节流的抽象基类：** InputStream,OutputStream
- **字符流的抽象基类：** Reader,Writer
```
注意：这四个类派生出来的子类名称都是以其父类名作为子类名的后缀。
    如：InputStream的子类FileInputStream
    如：Reader的子类FileRrader
```
## 字符流
- **定义：** 就是在字节流的基础上，加上编码，形成的数据流
- **字符流出现的意义：** 因为字节流在操作字符时，可能会有中文导致的乱码，所以由字节流引申出了字符流。

### FileWriter
**一、操作步骤**
1. 明确数据要存放的目的地。</br> 
　创建一个File对象，该对象一被初始化就必须要明确被操作的文件，并且该文件会被创建到指定目录下。如果该目录下已有同名文件，将会被覆盖。
2. 调用write方法，将字符串写入流中
3. 刷新(flush、close)流对象中缓冲的数据，将数据刷到目的地中
```
flush和close的区别：
1.close: 关闭流资源，但是关闭之前会刷新一次内部的缓冲中的数据
2.flush: 刷新后，流可以继续使用
```
**二、示例：(FileWriter的使用)**
```
package Writer;
import java.io.*;
public class FileWriterDemo {
	public static void main(String[] args) throws IOException
	{
		//创建file对象
		FileWriter f = new FileWriter("filewriterdemo.txt");
		//调用write方法
		f.write("abcd");
		//flush刷新
		f.flush();
		f.write("sad");
		//close刷新
		f.close();
	}
}
```
### IO异常的处理方式
**示例**
```
class FileWriteDemo2
{
    public static void main(String[] args)
    {
        FileWriter fw = null; //先创建引用变量，在创建对象
        try
        {
            fw.new FileWriter("demo.text");
            fw.write("abc");
        }
        catch(IOException e)
        {
             System.out.println(e.toString());
        }
        finally
        {
            try
            {
                if(fw!=null)
                    fw.close(); //用完后关闭资源
            }
            catch(IOException e)
            {
                System.out.printlb(e.toString());
            }
        }
    }
}
```
### 文件的续写
**示例：**
```
package Writer;
import java.io.*;
public class FileWriterDemo {
	public static void main(String[] args)
	{
		FileWriter fw = null;
		try 
		{
		    //传递一个true参数，代表不覆盖已有文件，并且在已有文件的末尾处进行数据续写
			fw = new FileWriter("filewriterdemo.txt",true);
			fw.write("d\r\nsa");    // \r\n在txt文本中表示换行
		}
		catch(IOException o)
		{
			System.out.println(o.toString());
		}
		finally
		{
			try
			{
				fw.close();
			}
			catch(IOException o)
			{
				System.out.println(o.toString());
			}
		}
	}
}

```

### 文本文件读取方式一
**一、使用步骤**
1. 创建一个文本读取流对象，和指定名称的文件相关联。</br>
    　要保证该文件是已经存在的，如果不存在，会发生异常FileNotFoundException
2. 调用读取流对象的read方法</br>
    　read():一个一个读取文件，而且会自动往下读。如果已达到流结尾，返回-1
3. 关闭资源</br>

**二、示例**
```
package Reader;
import java.io.*;
public class FileReaderDemo {
	public static void main(String[] args) throws IOException
	{
		FileReader fr = new FileReader("filewriterdemo.txt");
		int ch = 0;
		while((ch=fr.read())!=-1)
			System.out.print((char)ch);
	    fr.close();
	}
}

```
### 文本文件读取方式二：通过字符数组进行读取
**一、方法步骤**</br>
定义一个字符数组，用于存储读到的字符</br>
该read(char[])返回的是读到的字符个数</br>

**二、示例**
```
package Reader;
import java.io.FileReader;
import java.io.IOException;
import java.util.*;
public class FileReaderDemo1 {
	public static void main(String[] args) throws IOException
	{
		FileReader fr = new FileReader("filewriterdemo.txt");
		
		char[] buf = new char[1024];  //1024->2k，或者1024的整数倍
		int num = 0;
		while((num=fr.read(buf))!=-1)
		{
			System.out.println(new String(buf,0,num));
		}
		fr.close();
	}

}

```
### 拷贝文本文件
`如：将c盘的一个文本文件复制到D盘`</br>
**一、原理：**</br>
其实就是将c盘下的文件数据存储到D盘下的一个文件中。</br>
**二、步骤**                
1. 在D盘创建一个文件，用于存储c盘文件中的数据。
2. 定义读取流和c盘关联
3. 通过不断的读写完成数据存储
4. 关闭资源