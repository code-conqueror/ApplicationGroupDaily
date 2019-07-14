### 一、多态
1. 可以理解为事物存在的多种体现形态。
2. 多态的体现
    1. 父类的引用指向自己子类对象
    >Animal c = new Cat();  
    >Animal d = new Dog();

    2. 父类的引用也可以接受自己的子类对象
    >f(new Cat());  
    >public static void f(Animal a)  
    >{  
    >　　a.eat();  
    >}  
3. 多态的前提  
    　必须是类与类之间有关系，要么继承，要么实现</br>
    　通常还有一个前提：存在覆盖
4. 多态的好处：  
    　提高代程序的扩展性。
5. 多态弊端：  
    　只能用父类的引用访问父类的成员
6. 多态的应用
7. 多态使用的注意事项  
- 在多态中，非静态成员函数的特点：</br>
    - 在编译时期：**参阅引用型变量**所属的类中是否有调用的方法。如果有，编译通过，否则编译失败
    - 在运行时期：**参阅对象所属的类**中是否有调用的方法</br>
    （总结）成员函数在多态调用，**编译看左边，运行看右边**
- 在多态中，成员变量的特点：</br>
    - 无论编译和运行都，都**参考左边**（引用型便来你所属的类）
- 在多态中，静态成员函数的特点：
    - 无论编译和运行，**都参考左边**
### 二、向上转型/向下转型
1. 向上转型：子类转为父类
    >Animal c = new Cat(); //类型提升
2. 向下转型：强制将父类的引用转为子类类型
    >Animal c = new Cat();</br>
    >Cat c=(Cat a);</br>
    
    以下是错的
    >Animal c = new Animal();</br>
    >Cat c=(Cat a);</br>
    
    **`不能将父类对象转成子类类型，多态自始至终都是子类对象在做变化`**</br>
3. **instanceof**关键字
    - 用法：`A instanceof B`</br>
    　A 类型转换为 B 类型</br>
    例：
    ```
    Animal a = new Cat();
    if(a instanceof Cat)
    {
        Cat c = (Cat)a;
    }
    ```
### 三、Object
1. Object:是所有对象的直接或间接父类，定义的是所有对象都具备的功能。
典例：
```
package Pac1;

public class ObjectDemo {
	public static void main(String[] args) {
		Demo d1 = new Demo(4);
		Demo d2 = new Demo(4);
		System.out.println(d1.equals(d2));
	}
}
class Demo{
	private int num;
	Demo(int num)
	{
		this.num = num;
	}
	public boolean equals(Object obj)
	{
		Demo d = (Demo)obj;
		return this.num==d.num;
	}
}
运行结果：
ture
```
### 四、内部类
1. 定义：将一个类定义在另一个类的里面，对里面那个类就称为内部类（内置类、嵌套类）
2. 访问特点：
    - 内部类可以直接访问外部类的中的成员，包括私有成员</br>
    　==之所以可以直接访问外部类中的成员，是因为内部类持有一个外部类的引用,格式为：外部类名+this==
    - 外部类要访问内部类中的成员，必须建立内部类的对象
3. 访问格式
    - 当内部类定义在外部类的成员位置上，且非私有，可以在外部其他类中直接建立内部类对象  
    　　外部类名.内部类名 对象名 = new 外部类().new 内部类();</br> 
	　　Outer.Inner in = new Outer().new Inner();
	in.show();
    - 当内部内在成员位置上，就可以被成员修饰符修饰。</br>
    1. 如:private:将内部类在外部类中进行封装</br>
    　static:内部类就具备static特性
        - 当内部类被static修饰时，只能直接访问外部类中的static成员。出现访问局限
        - 在外部其他类中，如何直接访问static内部类的非静态成员呢？</br>
        　new Outer.Inner().show();</br>
        - 在外部其他类中，如何直接访问static内部类的静态成员呢？</br>
        　Outer.Inner.show();</br>
    2. 注意：当内部类中定义了静态成员，该内部类必须是static.
    3. 当外部类中的静态方法访问内部类，内部类也必须是静态的
    4. 注意：当内部类在外部类的成员位置上时,内部类可以被私有修饰
4. 应用：  
　当描述事物时，事物的内部还有事物，该事物用内部类来描述。因为内部事务在使用外部事物的内容
```
package Pac1;
/**
 * 内部类的使用形式
 * @author code-conqueror
 *
 */
public class InnerClassDemo {
	public static void main(String[] args) {
		//访问内部类中的成员函数
		Outer out = new Outer();
		out.method();
		//直接访问内部类中的成员,格式：外部类名.内部类名 对象名 = new 外部类().new 内部类();
		Outer.Inner in = new Outer().new Inner();
		in.show();  
	}

}
class Outer{
	private int num=5;
	class Inner{
		int num = 6;
		public void show()
		{
			int num =3;
			System.out.println("num="+num);			//3
			System.out.println("this.num="+this.num);		//6
			System.out.println("Outer.this.num="+Outer.this.num);		//5
		}
	}
	public void method()
	{
		Inner i = new Inner();
		i.show();
	}
}
运行结果：
num=3
this.num=6
Outer.this.num=5
num=3
this.num=6
Outer.this.num=5
```
### 五、匿名内部类
1. 匿名内部类其实就是内部类的简写格式。
2. 定义匿名内部类的前提：</br>
    - 内部类必须是继承一个类或者实现接口
3. 匿名内部类的格式:</br>
    new 父类或者接口{}(定义子类的内容)
4. 匿名内部类就是一个`匿名子类对象`,带内容的对象。
5. 匿名内部类方法不最好不超过三个
```
package Pac1;
/**
 * 匿名内部类的使用形式
 * @author code-conqueror
 *
 */
public class InnerClassDemo {
	public static void main(String[] args) {
		Outer out = new Outer();
		out.function();
	}
}

abstract class AbsDemo{
	abstract void show();
}
class Outer{
	int x = 3;
//	class Inner{
//			void show()
//			{
//				System.out.println("show :"+x);
//			}
//	}
	public void function()
	{
		//new Inner().show();
		new AbsDemo()   //匿名内部类
		{
			void show()
			{
				System.out.println("x==="+x);
			}
		}.show();
	}
}

```
### 六、异常
1. 就是程序正在运行时出现不正常情况，**对问题进行描述，封装成对象**
    - 好处：
        - 见问题进行封装
        - 将正常流程代码和问题处理代码相分离，方便阅读
2. 对问题的划分
    1. 严重的问题
        - 对于严重的，java通过Erroe类进行描述。对Error一般不编写针对性的代码对其进行处理
    2. 非严重的问题
        - 对于非严重的，java通过Expection类进行描述，对于Expection都可以使用针对性地处理方式进行处理。
3. 异常的体系：
```
Throwable
    |--Error
    |--Exception
        |--RuntimeException
```
4. 无论Error或者Expection都有一些共性的内容,函数有异常发生，函数就结束了
5. 异常的处理：  
java提供了特有的语句进行处理
```
try
{
    需要被检测的代码;
}
catch(异常、变量)
{
    处理异常的代码;（处理方式）
}
finally
{
    一定执行的语句;
}
``` 
6. 对捕获的方法进行常见方法操作</br>
    String getMessage();    //异常信息</br>
    String toString();     /by zero //异常名称 异常信息</br>
    void printStackTrace();
    //异常名称 异常信息 异常出现的位置
7. jvm默认的异常处理机制，就是在调用printStackTrace方法，打印异常堆栈的
8. throws关键字
    - 通过throws关键字声明可能会出现的问题
    - throws Exception</br>
        可有两种形式解决，捕捉或者抛出去</br>
        1. 调用的时候抛出去,抛给jvm处理，throws Exception
        2. 捕捉：catch(Exception e){
        }
9. 对多异常处理
    1. 声明异常时，建议声明更为具体的异常,这样处理可以更具体
        - 如:throws ArithmeticException
    2. 对方声明几个异常，就对应有几个catch块，不要定义多余的catch块</br>
    　如果多个catch块中的异常出现继承关系，父类异常catch快放在最下面
10. 建议在进行catch处理时，catch中一定要定义具体的处理方式。

### 七、自定义异常
1. 定义在继承Exception或者RuntimeException
    - 为了让该自定义类具备可抛性
    - 让该类具备操作异常的共性方法
2. 格式：
```
class MyException() extends Exception
{
    MyException(String e)
    {
        super(e);
    }
}
```
3. 自定义异常，必须是自定义类继承Exception  
原因：异常体系有一个特点，因为异常类和异常对象都被抛出他们都具备可抛性。这个可抛性是        Throwable这个体系操作，只有这个体系中的类和对象才可以被throw和throws操作
4. **throws和throw的区别：**</br>
    1.throws 使用在函数上</br>
    　throw使用在函数内</br>
    2.throws后面跟着是一个异常类。可以是多个，用逗号隔开</br>
    　throw后跟的是异常对象</br>
5. Exception中有一个特殊的子类异常RuntimeException运行时异常</br>
    - 如果在函数内抛出异常，函数上就可以不用声明，编译通过
    - 如果在函数上声明异常，调用者可以不用进行处理，编译通过
6. 自定义异常时，如果该异常的发生，无法再继续进行运算，就让自定义异常继承RuntimeException.
7. 对于异常分两类：</br>
    1. 编译时异常
        - 该异常在编译时，如果没有处理（没有抛出，也没有try），编译失败
        - 该异常被标识，代表这可以被处理
    2. 运行时异常（RuntimeException以及其子类)
        - 在编译时，不需要处理，编译器不检查。
        - 该异常的发生，建议不处理，让程序停止。需要对代码进行修正。