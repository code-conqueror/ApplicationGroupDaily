### 异常
 - 典例：
 ```
 package pro2;

/**
 * 异常--练习1
 * @author code-conqueror
 *
 */
/*问题：蓝屏、冒烟*/
//蓝屏  
class LanPingException extends Exception
{
	LanPingException(String message)
	{
		super(message);
	}
}
//冒烟
class MaoYanException extends Exception
{
	MaoYanException(String message)
	{
		super(message);
	}
}
class NoPlanException extends Exception
{
	NoPlanException(String message)
	{
		super(message);
	}
}
class Computer{
	private int state = 3;		//状态
	public void run() throws LanPingException,MaoYanException
	{
		if(state == 2)
			throw new LanPingException("电脑蓝屏");
		if(state == 3)
			throw new MaoYanException("电脑冒烟");
		System.out.println("电脑正常运作");
	}
	public void reset()
	{
		System.out.println("电脑重启");
	}
}
class Teacher{
	private String name;
	private Computer cmpt;
	Teacher(String name)
	{
		this.name=name;
		cmpt = new Computer();
	}
	public void prelect() throws NoPlanException
	{
		try
		{
			cmpt.run();
		}
		catch(LanPingException e)
		{
			cmpt.reset();
		}
		catch(MaoYanException e)
		{
			test();
			throw new NoPlanException("课时无法继续"+e.getMessage());
		}
		System.out.println("讲课");
	}
	public void test()
	{
		System.out.println("学生做练习");
	}
}
public class ExceptionTest {

	public static void main(String[] args) 
	{
		Teacher t = new Teacher("毕老师");
		try
		{
			t.prelect();
		}
		catch(NoPlanException e)
		{
			System.out.println(e.toString());
			System.out.println("换老师或者放假");
		}
	}
}
运行结果：
学生做练习
pro2.NoPlanException: 课时无法继续电脑冒烟
换老师或者放假
 ```
 ### finally
 1. 定义：一定会被执行的代码，**通常用于关闭资源**  
    　finally只有一种请况不会执行，当执行到**System.exit(0);**　finally不会执行
 2. 如：
 ```
 void method()
 {
    try{}
    catch{}
    finally{A语句}
    B语句
 }
```
`一般情况下，A、B语句都会被执行,但是catch的语句有"return"语句，A会被执行，而B不会被执行，而出现“System.exit(0);//系统退出，jvm结束”语句，A和B都不执行。`
3. 对于数据库的操作：
```
    try
    {
        连接数据库；
        数据库操作；//throw new SQLException();
    }
    catch(SQLException e)
    {
        会对数据库进行异常处理
    }
    finally
    {
        关闭数据库；
    }
``` 
4. 格式：   
- 格式一：
>try{}  
>catch{}  
>finally{}  

- 格式二：
>try{}  
>catch{}

- 格式三：
>try{}  
>finally{}  
 
5. 一些情况分析：
```
1.  void method()
    {
        try{
            throw new Exception();
        }
    }
    编译异常，此时在函数上加throws Exception来抛出异常
        void method() throws Exception(){...}
2.  void merhod()
    {
        try{
             throw new Exception();
        }
        catch(Exception e){
            ...
        }
    }
    正确，catch会接受异常并在内部解决
3.  void method()
    {
        try{
            throw new Exception();
        }
        finally{
            //关闭资源
        }
    }
    编译异常，此时在函数上加throws Exception来抛出异常，finallyn内可以加一些代码，或者关闭资源
```

5. 异常在在子父类覆盖中的体现：
    1. 子类在覆盖父类时，如果父类的方法抛出异常时，那么子类的覆盖方法，只抛出父类的异常或者该异常的子类，**子类不能抛出新的异常**
    2. 如果父类方法抛出多个异常，那么，**子类在覆盖该方法时只能抛出父类异常子集**
    3. 如果父类或者接口的方法中没有异常抛出，那么子类在覆盖方法是，也不可以抛出异常。</br>
        **如果子类方法发生异常，就必须进行try处理，绝对不能抛出**  
- 典例：
```
package pro2;
/**
 * 异常--练习2
 * @author code-conqueror
 *
 */

public class ExceptionTest2 {
	public static void main(String[] args)
	{
		/*
		try
		{
			Circle c = new Circle(-1);
			c.getArea();
		}
		catch(NoValuesException e)
		{
			System.out.println(e.toString());	//打印问题的名称和问题信息
		}
		*/
		/*若输出出现非法值，求面积是没有意义的，此时把Excepton改为RuntimeException即可
		,此时运行会出现错误，提醒用户更改数值*/
h24->   Circle c = new Circle(-2);
		c.getArea();
	}
}
//class NoValuesException extends Exception
class NoValuesException extends RuntimeException
{
	NoValuesException(String message)
	{
		super(message);
	}
}
interface Shape
{
	void getArea();
}
class Circle implements Shape
{
	private double radius;
	public static  final double PI = 3.14;
	Circle(double radius)	//throws NoValuesException
	{	
		if(radius<0)
			throw new NoValuesException("输入出现非法值");
		this.radius=radius;
		
	}
	public void getArea()		//子类权限大于父类权限
	{
		System.out.println(radius*radius*PI);
	}
}
运行结果：
更改前：
    pro2.NoValuesException: 输入出现非法值

更改后：
    Exception in thread "main" pro2.NoValuesException: 输入出现非法值
	at pro2.Circle.<init>(ExceptionTest2.java:47)
	at pro2.ExceptionTest2.main(ExceptionTest2.java:24)

```

### 做题总结：
1. 子类用构造函数进行初始化是，构造函数的在第一句隐藏super();
2. 变量不存在覆盖，随类走的
3. throw单独存在，下面不要定义语句，因为执行不到。若有语句，着编译失败。
```
try
{
    throw ...
    System.out.println("a");
}
编译失败，打印"a"语句执行不到
```
4. 主函数是静态的，如果访问内部类需要被static修饰
5. 多个catch时，父类的catch要放在下面        