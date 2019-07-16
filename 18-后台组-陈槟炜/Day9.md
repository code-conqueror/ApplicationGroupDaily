### 一、多线程
1. **进程：** 是一个正在执行中的程序。</br>
  每个进程执行都有一个执行顺序，该顺序是一个执行路径,或者叫一个控制单元
2. **线程：** 就是进程中的一个独立的控制单元。线程控制着进程的执行
3. 一个进程至少有一个线程
4. **主进程：**
    1. 进程中至少一个线程负责java程序的执行
    2. 线程运行的代码存在于main方法中
5. **拓展：**
    jvm启动不止一个线程，还有负责垃圾回收的线程
6. **多线程存在的意义：** 提高运行效率
7. **创建线程得第一种方式**：继承Thread类</br>
    步骤：
    1. 定义类继承Thread
    2. 复写Thread类中的run方法</br>
        `目的：将自定义代码存储在run方法，让线程运行`
    3. 调用线程的start方法</br>`start方法有两大作用：启动线程，调用run方法` <br>
    
```
class Demo extends Thread    //创建Thread的子类
{
    public void run()   //重写run方法
    {
        //线程要执行的代码
    }
}
class ThreadDemo
{
    public static void main(String[] args)
    {
        Demo d = new Demo();
        d.start();  //开启线程并执行该线程的run方法
        d.run();    //仅仅是对象调用方法，线程创建了但没有运行
    }
}
```
8. **多线程的特性：** 随机性</br>
    多个线程都获取cpu的执行权，cpu执行到谁，谁就执行 
9. **多项程的状态**
```
graph LR
A[被创建]-->|start|B(运行)
B-->|stop<>,run方法结束|E(消亡)
B-->D(阻塞)
C-->D(冻结等待)
B-->|1.sleep<time>|C
C-->|1.sleep时间到了|B
B-->|2.wait<>|C
C-->|2.nitify|B
```
10. 线程都有自己的默认的名称</br>
    Thread-编号 该编号从0开始
11. Thread类内的函数：</br>
    1. currentThread()返回当前线程对象</br>
    2. getName():获取线程名称</br>
12. **创建接口的第二种方式：** 实现Runnable接口</br>
    步骤：
    1. 定义类实现Runnale接口
    2. 覆盖Runnable接口中的run方法
    3. 通过Thread类建立线程对象
    4. 将Runnable接口的子类对象作为实际参数传递给Thread类的构造函数</br> 
- 典例：
```
package package2;
/**
  *  购票系统（多窗口买票）
 * @author code-conqueror
 *
 */
class Ticket implements Runnable		//实现Runnable接口
{
	private int ticket=100;
	public void run()					//覆盖run()
	{
		while(ticket>0)
			System.out.println(Thread.currentThread().getName()+" "+ticket--);
	}
	
}
public class TicketDemo {
	public static void main(String[] args)
	{
		Ticket t = new Ticket();		
		Thread t1 = new Thread(t);		//建立Thread类对象，并将t作为参数代入
		Thread t2 = new Thread(t);
		Thread t3 = new Thread(t);
		Thread t4 = new Thread(t);
		t1.start();
		t2.start();
		t3.start();
		t4.start();
	}
}
```
13. **实现方式和继承方式的区别**</br>
    - **实现方式的好处**:避免了单继承的局限性,在定义线程时，建议使用实现方式
    - **线性代码存放的位置不同：**
        - **继承Thread:** 线程代码存放在Thread子类run方法中
        - **实现Runnable:** 线程代码存放在接口子类的run方法中
14. 多线程的安全问题：</br>
    1. **原因：**</br>
    当多条语句在操作同一个线程共享数据时，一个线程对多条语句只执行了一部分，还没有执行完，另一个线程就参与进来执行，导致共享数据的错误
    2. **解决办法**</br>
    对多条操作共享数据的语句，只能让一个线程都执行完，在执行过程中，其他线程不可以参与执行
15. **同步代码块**</br>
- 格式为
```
synchronized(对象)
{
    需要被同步的代码;
}
//对象相当于锁，只有锁的进程可以在同步中执行。没有锁的线程即使获取cpu的执行权也进不去
```
- 同步的前提：
    1. 必须要有两个或者两个以上的线程
    2. 必须是多个线程使用同一个锁
- 好处：解决多线程的安全问题
- 弊端：多个线程需要判断锁，较为消耗资源
16. **同步函数:** 
- **定义：** 把`synchronized`作为修饰符放在函数上
- 同步函数的锁是this</br>
    函数需要被对象调用，那么函数都有一个所属对象的引用`this`
17. **同步函数被static(静态)修饰时**，所用的锁是该方法所在类的字节码文件对象，也就是**类名.class**
18. **死锁**
    多个线程因竞争资源而造成的一种僵局（互相等待），若无外力作用，这些进程都将无法向前推进。