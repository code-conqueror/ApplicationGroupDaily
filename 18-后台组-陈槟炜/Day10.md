### 一、多线程
1. 典例：
```
package package1;

class Res
{
	private String name;
	private String sex;
	private boolean flag = false;
	public synchronized void set(String name,String sex)
	{
		if(flag)
			try{this.wait();} catch(Exception e) {}
		this.name = name;
		this.sex = sex;
		flag = true;
		this.notify();
	}
	public synchronized void out()
	{
		if(!this.flag)
			try{this.wait();} catch(Exception e) {}
		System.out.println(this.name+"...."+this.sex);
		this.flag =false;
		this.notify();
	}
}
class Input implements Runnable
{
	private Res r;
	Input(Res r)
	{
		this.r=r;
	}
	public void run()
	{
		int x=0;
		while(true)
		{
				if(x==0)
					r.set("mike","man");
				else
					r.set("李丽","woman");
				x=(x+1)%2;
		}
	}
}
class Output implements Runnable
{
	private Res r;
	Output(Res r)
	{
		this.r=r;
	}
	public void run()
	{
		while(true)
		{
				r.out();
		}
	} 
}
public class ThreadDemo {
	public static void main(String[] args)
	{
		Res r=new Res();
		
		Input in = new Input(r);
		Output out = new Output(r);
		
		Thread t1 = new Thread(in);
		Thread t2 = new Thread(out);
		t1.start();
		t2.start();
	}
}
运行结果
李丽....woman
mike....man
李丽....woman
mike....man
李丽....woman
mike....man
...

```
2. notify() : 唤醒第一个线程
3. notifyAll() :全部唤醒所有线程池的线程
4. 解决多生产者消费者的问题，用while循环和notifyAll解决问题
5. 为什么要定义while判断标记</br>
    原因：让被唤醒的线程再一次判断标记
6. 为什么要定义notifyAll</br>
    原因：因为要唤醒对方线程。只用notify,同意出现只唤醒本方线程的情况，导致程序中的所有线程都等待
7. 现实LocK操作替换同步Synchronized</br>将Object中的wait,notify,notifyAll，替换了Condition对象
### 二、停止线程
1. **定义循环结束标记**
    - 因为线程运行代码一般都是循环，只要控制了循环即可
2. **使用interrupt(中断)方法**
    - 该方法时结束进程的冻结状态，使线程回到运行状态中来
3. ==shop方法已经过时，不能使用==
4. interrupt使处于冻结状态的线程**强制**恢复为运行状态
5. 当没有指定的方式让冻结的线程恢复到运行状态时，这是需要对冻结状态进行清除，强制让线程恢复到运行前的状态。这样就可以操作标记让线程结束，Thread类提供的方法为interrupt()
### 三、守护线程（setDaemon)
1. **格式：** </br>
public void setDaemon(boolean on)
2. **使用条件：**</br> 正在运行的线程都是守护线程时，jvm退出，该方法必须在启动线程前调用
### 四、join方法
1. 当A线程执行到B线程的.join()方法时，A就会等待。等到B线程都执行完后，A才会执行。
2. join可以用来临时加入线程执行
```
如：
class JoinThreadDemo
{
    public static void main()
    {
        创建线程A；
        A.start();
        A.join();
    }
}
主线程执行到A线程时，A线程会夺取cpu执行权，运行线程A，主线程冻结，知道A线程结束后才运行主线程
```
### 五、改变线程的优先级
- **方法：** 调用setPriority()函数</br>
    如：t1.setPriority(Thread.MAX_PRIORITY);
- 其中优先级分为10级，级数高的会优先执行,默认优先级为5级
- MAX_PIORITY ->10
- MIX_PIORITY ->1
- NORM_PRIORITY ->5
### 六、String类
#### String类概述
1. **创建对象的方式:**</br>
    1. String s1 = "abc";  //s1是一个类类型变量，"abc"是一个对象
    2. String s2 = String("abc");
2. 字符串最大的特点是：一旦被初始化就不可以改变。
```
    String s1 = "abc";
    s1 = "sad";
    System.out.println(s1);//打印sad
原因：字符串没有改变，改变的是s1的指向内容
```
3. **String str = "abc"创建对象的过程**</br>
    > １．首先在常量池中查找是否存在内容为"abc"字符串对象</br>
    > ２．如果不存在则在常量池中创建"abc"，并让str引用该对象</br>
    > ３．如果存在则直接让str引用该对象</br>

4. **String str = new String("abc")创建实例的过程**</br>
    > １．首先在堆中（不是常量池）创建一个指定的对象"abc"，并让str引用指向该对象</br>
    > ２．在字符串常量池中查看，是否存在内容为"abc"字符串对象</br>
    > ３．若存在，则将new出来的字符串对象与字符串常量池中的对象联系起来</br>
    > ４．若不存在，则在字符串常量池中创建一个内容为"abc"的字符串对象，并将堆中的对象
    > 之联系起来</br>
5. **总结**
    - String str = "abc" ，创建一个对象。
    - String str = new String("abc")创建两个对象，若存在“abc",则只创建一个对象
6. 如：
```
1.  String s1 = "abc";
	String s2 = "abc";      
	System.out.println(s1==s2);         //true
	System.out.println(s1.equals(s2));  //true，比较的值
	//字符串变量已经在内存中存在，所以s2发现s1已经存在所以不会独立开辟空间，即s1==s2
	
2.	String s1 = new String("abc");
	String s2 = new String("abc");
	System.out.println(s1==s2);         //false
	System.out.println(s1.equals(s2));  //true
	
3.  String s1 = "abc";
    String s2 = new String("abc");
	System.out.println(s1==s2);         //false
	System.out.println(s1.equals(s2));  //true
```

#### 常见功能
1. **获取**</br>
- **字符串的长度**  
        int length();
- **根据位置获取某个字符**
        char charAt(int index)
- **根据字符获取该字符在字符串中的位置** 
    1. int indexOf(int ch) //返回的ch在第一次出现的位置 
    2. int indexOf(int ch,int fromIndex)  //从fromIndex指定的位置开始，获取ch在字符串中出现的位置  
    3. int indexOf(String str) //返回的str在第一次出现的位置,没找到返回-1
    4. int indexOf(String str,int fromIndex)  //从fromIndex指定的位置开始，获取str在字符串中出现的位置  
    5. int lastindexOf(String str)
        //反向索引str第一次出现的位置，返回的位置是从前开始数的
```
    String s2 = "abGaaHhjhFc";
	System.out.println(s2.indexOf("a"));        //0
	System.out.println(s2.lastIndexOf("a"));    //4
	System.out.println(s2.indexOf("Ga"));       //2
	System.out.println(s2.lastIndexOf("Ga"));   //2
```
 
2. **判断**
- **判断字符串中是否包含某个子串**  
        boolean contains(str);  
        //int indexOf(String str)
- **字符串是否有内容**  
        boolean isEmpty();
- **字符串是否以指定内容开头**  
        boolean startsWith(str);
- **字符串是否以指定内容结尾**  
        boolean endsWITH(str);
- **判断字符串内容是否相同**  
        boolean equals(str);
- **判断内容是否相同，并忽略大小写**  
        bool equalsIgnoreCase();
3. **转换**
- **将字符数组转成字符串**  
    （一）构造函数：  
    String(char[])  
    String(char[],offset,count):将字符数组中的一部分转成字符串  
    （二）静态方法：  
    static String copyValueOf(char[]);  
    static String copyValueOf(char[] data,int offset,int count);
    static String ValueOf(char[]);
- **将字符串转成字符数组**  
    char[] toCharArray();
- **将字节数组转成字符串**  
    String(byte[])
    String(byte[],offect,count):将字节数组中的一部分转成字符串  
- **将字符串转成字节数组**  
    byte[] getBytes()
- **将基本数据类型转成字符串**
    static String valueOf(int);
    static String valueOf(double);
4. **替换**  
- String replace(oldchar,newchar);  
    如果替换的字符不存在，返回的是原字符串
5. **切割**  
- String[] split(regex);//regex->分割符
6. **子串：** 获取字符串的一部分
- String substring(begin); //从指定位置到结尾
- String substring(begin,end);  //包含头，不包含尾
7. **转换、去除空格、比较**
- **将字符串转成大写或者小写
    String toUpperCase();
    String toLowerCase();
- 将字符串两端的多个空格去除
    String trim();
- 对两个字符串进行自然顺序的比较
    int compareTo(String); 