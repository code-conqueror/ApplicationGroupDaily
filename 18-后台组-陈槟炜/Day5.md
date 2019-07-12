### 一、java帮助文档（API文档）的创建
1. java的使用说明书通过文档注释来完成
2. 如：
    >/**  
    >@author  创建者   
    >@version  版本号  
    >@param 说明的变量 说明用途  
    >@return 返回的内容  
    >*/
3. 类要修饰为共有的(public)，构造函数设为私有的
4. 要用到bin目录下的javadoc
    - javadoc -d 文件夹名 (-author -version)  类名.java
    - 打开文档的index目录
5. 默认构造函数的权限随类的变化而变化自己创建空参数构造函数不是默认构造函数
### 二、静态代码块
1. 格式：  
    　　static  
    　　{  
    　　　　静态代码块执行语句  
    　　}
2. 特点：随类的加载而执行，只执行一次，并优先于主函数  
    用于==给类进行初始化==
3. 典例：
```
package pro2;
/**
 * 
 * @author code-conqueror
 * @version 1.0
 */
public class StaticCodeDemo {
	static		//静态代码块初始化，对“类”进行初始化，只运行一次
	{
		System.out.print("a ");
	}
	public static void main(String[] args)
	{
		new StaticCode();
		new StaticCode();	//随类的加载而执行，只执行一次 
		System.out.println("over!");
		StaticCode.show();
		System.out.println("*********");
		StaticCode a=null;
	}
	static
	{
		System.out.println("b");
	}
}
class StaticCode{
	static
	{
		System.out.println("静态代码块初始化，对“类”进行初始化，只运行一次");
	}
	public static void show()
	{
		System.out.println("run");
	}
	{			//构造代码块初始化，对“对象”进行初始化
		System.out.println("构造代码块初始化，对“对象”进行初始化");
	}
	StaticCode()
	{
		System.out.println("构造函数初始化，对”对应的对象“初始化");
	}
}


运行结果：
a b
静态代码块初始化，对“类”进行初始化，只运行一次
构造代码块初始化，对“对象”进行初始化
构造函数初始化，对”对应的对象“初始化
构造代码块初始化，对“对象”进行初始化
构造函数初始化，对”对应的对象“初始化
over!
run
*********
```
### 三、设计模式
1. **定义**：设计模式（Design Pattern）是前辈们对代码开发经验的总结，==是解决特定问题的一系列套路==。它不是语法规定，而是一套用来提高代码可复用性、可维护性、可读性、稳健性以及安全性的解决方案。
2. java中有23种设计模式
    - **单例设计模式**：解决一个类在内存中只存在一个对象  
        1. 想要保证对象唯一
            - 为了避免其他程序过多建立该类对象，先禁止其他程序建立该类对象
            - 为了让其他程序可以访问到该类对象，只好在本类中，自定义一个对象
            - 为了方便其他程序对自定义对象的访问，可以对外提供一些访问方式
        2. 体现
            - 将构造函数私有化
            - 在类中创建一个私有并静态的本类对象
            - 提供一个方法可以获取该对象
        3. 应用  
         当需要将事物的对象保证在内存中唯一时，就将以上三步加上即可
    - 模板方法设计模式  
        1. 在定义功能时，功能的一部分是确定的，但是还有一部分是不确定的，而确定的部分在使用不确定的部分，这是就将不确定的部分暴露出去
        
```
package pro2;
/**
 * 单例设计模式
 * @author code-conqueror
 *
 */
class SingleDemo
{
	public static void main(String[] args)
	{
		//Single s1=new Single();	出错，构造函数是私有的
		//Single s2=new Single();
		Single s1 = Single.getInstance();	//s1指向s
		Single s2 = Single.getInstance();	//s2指向s
		s1.setNum(23);
		System.out.println(s2.getNum());
	}
}
class Single
{
	private int num;
	public void setNum(int num)
	{
		this.num=num;
	}
	public int getNum()
	{
		return num;
	}
	private Single() {}			//将构造函数私有化，避免其他程序过多建立新对象
	private static Single s = new Single();	//在类中创建一个私有并静态的本类对象
	public static Single getInstance()		//提供一个方法可以获取对象
	{
		return s;
	}
}
运行结果
23 
```
### 四、继承
1. 优点：
    - 提高代码的复用性
    - 让类与类产生关系，有关系才有多态  
2. 关键字：**extends**
3. java中,java只支持单继承，不支持多继承。  
    　因为多继承容易带来安全隐患；当多个父类中定义了相同功能，当内容不同是，不确定要运行哪一个。  
    　但是java保留了这种机制，斌用印一种形式来体现，多实现
4. java支持多层继承。也就是一个继承体系 
5. 查阅父类的功能，创建子类对象使用的功能
6. **super**关键字表示父类（超类）。
7. 如果子类中出现非私有的同名成员变量时，子类要访问本类对象用“this”，访问父类对象用“super”
8. 当子类出现和父类一模一样的函数时，当子类对象调用该函数时，会运行子类函数的内容，这种情况叫“**覆盖**”
9. 覆盖
    - 子类覆盖父类，必须保证子类权限大于父类权限，否则编译失败  
        权限：public > 默认权限 > private , protected
    - 静态只能覆盖静态
10. **重写和重载的区别：**
    - 重载：只看参数列表
    - 重写：子符类方法要一模一样
11. 子父类中构造函数  
    - 在对子类对象进行初始化时，父类对象也会运行，那是因为子类的构造函数默认第一行有一条隐式语句**super();**
    - 子类一定要访问父类的构造函数
    - super语句一定要放在构造函数的第一行
    - **结论**
    1. 子类的所有构造函数，默认都会访问父类中控参数的构造函数
    2. 当子类中没有空参数的构造函数时子类必须手动通过super语句来指定要访问父类中的构造函数
    3. 子类构造函数的第一行可以手动指定this来访问本类中的构造函数
    - 注意：this和super不能同时放在第一行，只能择其一。
12. **final**：最终
    - 可以修饰类、函数、变量
    - **被final修饰的类不能被继承**。为了避免被继承，被子类覆写功能
    - **被final修饰的方法不能被复写**
    - **被final修饰的变量是一个常量**只能赋值一次，即可以修饰成员变量，也可以修饰局部变量
        - 规范：常量的书写都大写，单词间连接用_
    - 内部类定义在类中的局部变量时，只能访问被final修饰的局部变量
13. 当多个类中出现相同的功能，但是功能主体不同，可以进行向上抽取，这时，只能抽取功能定义，而不能抽取功能主体
14. **抽象类**
    - 特点：
        - 抽象方法一定在抽象类中
        - 抽象方法和抽象类都必须被**abstract**关键字修饰
        - 抽象类不能用 new创建对象
        - 抽象类中的抽象方法要被使用，必须由所有子类复写所有的抽象方法后，建立子类对象调用。  
        　如果子类只覆盖了一部分抽象方法，那么该子类还是抽象类
    - **抽象类可以不定义方法**，这样做仅仅是不让抽象类建立对象
### 五、接口
1. 可以认为是一个特殊的抽象类
2. 当抽象类中的方法都是抽象的，那么该类可以通过接口的形式来表示
3. **interface** 关键字用于定义接口
4. 接口中的成员都有固定修饰符
    - 常量：**public** static final
    - 方法：**public** abstract
5. implements代替extends
6. 接口是不可以创建对象的，因为有抽象方法，需要被子类实现，只有子类对接口中的抽象方法全部覆盖后，子类才可以实例化，否则子类是一个抽象的
7. 典例
```
package pro2;
/**
 * 
 * @author code-conqueror
 *
 */
public class InterfaceDemo {
	public static void main(String[] args)
	{
		Test t = new Test();
		System.out.println(t.NUM);
		System.out.println(Test.NUM);
		System.out.println(Inter.NUM);
	}
}
interface Inter
{
	public static final  int NUM = 3;
	public abstract void show();
}
class Test implements Inter{
	public void show() {}
}
运行结果：
3
3
3
```
8. **接口可以被类多实现，也是对多继承不支持的转换形式，Java支持多实现**
9. **只有在接口与接口间存在多继承**
```
interface A
{
	void methodA();
}
//接口与接口间可以用继承
interface B	//extends A,
{
	void methodB();
}
interface C extends B,A
{
	void methodC();
}
//接口与接口间可以用实现
class D implements C
{
	public void methodA(){}
	public void methodB(){}
	public void methodC(){}
}
//类可以继承一个类的同时实现多个接口
class E extends D implements A,B
{
    
}
```
10. **（总结）接口的特点**
    1. 接口是对外暴露的规则
    2. 接口是程序的功能拓展
    3. 接口可以用来多实现
    4. 类和接口是多实现关系，而且类可以继承一个类的同时实现多个接口
    5. 接口与接口间可以有继承关系