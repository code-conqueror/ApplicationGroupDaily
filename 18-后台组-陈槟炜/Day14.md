[toc]
# 集合框架
## treeSet
### 一、特点
- 可以对Set集合中的元素进行排序。
- 底层数据结构是二叉树
- 保证元素唯一性的依据：
    - compareTO方法return 0;

### 二、TreeSet排序的方式

#### TreeSet排序的第一种方式：让元素自身具备比较性
    - 元素需要实现Comparable接口，覆盖compareTo方法。
#### TreeSet排序的第二种方式：让集合自身具备比较性
    - 当元素自身不具备比较性时，或者具备的比较性不是我们所需要的，这时就需要让集合（容器）自身具备比较性。
    - 定义比较器，将比较器对象作为参数传递给TreeSet集合的构造函数
    - 定义一个类，实现Comparator接口，覆盖compare方法。
    
- **注意：** 
- 当主要条件相同时一定要判断次要条件
- 当两种排序都存在时，以比较器为主
### 三、示例1：TreeSet用法（排序方法一）
```
package collection;
import java.util.*;
/**
 * TreeSet的用法（按照年龄排序）
 * @author code-conqueror
 *
 */
public class TreeSetDemo {
	public static void main(String[] args)
	{
		TreeSet ts = new TreeSet();
		ts.add(new Student("java01",11));
		ts.add(new Student("java02",10));
		ts.add(new Student("java04",14));
		ts.add(new Student("java08",14));
		ts.add(new Student("java04",14));

		Iterator it =ts.iterator();
		while(it.hasNext())
		{
			Student stu = (Student)it.next();
			sop(stu.getName()+"::"+stu.getAge());
		}
	}
	public static void sop(Object obj)
	{
		System.out.println(obj);
	}
}
class Student implements Comparable //该接口强制让学生具备比较性
{
	private String name;
	private int age;
	Student(String name,int age)
	{
		this.name=name;
		this.age=age;
	}
	public int compareTo(Object obj)
	{
		if(!(obj instanceof Student))
			throw new RuntimeException("不是学生对象");
		Student s = (Student)obj;
		System.out.println(this.name+"...compare.."+s.name);
		if(this.age > s.age)
			return 1;
		if(this.age == s.age)
			return this.name.compareTo(s.name);
		return -1;
	}
	public String getName() 
	{
		return name;
	}
	public int getAge()
	{
		return age;
	}
}
//classCastException:类型转换异常
运行结果：
java01...compare..java01
java02...compare..java01
java04...compare..java01
java08...compare..java01
java08...compare..java04
java04...compare..java01
java04...compare..java04
java02::10
java01::11
java04::14
java08::14

```
### 四、示例2：TreeSet用法（排序方法二）
```
package collection;
import java.util.*;
/**
 * TreeSet用法（排列方式二）
 * @author code-conqueror
 *
 */
public class TreeSetDemo1 {
	public static void main(String[] args)
	{
		TreeSet ts = new TreeSet(new MyCompare());   //创建比较器对象
		ts.add(new Student("java01",11));
		ts.add(new Student("java02",10));
		ts.add(new Student("java04",14));
		ts.add(new Student("java08",14));
		ts.add(new Student("java04",15));
		ts.add(new Student("java02",10));
		
		Iterator it =ts.iterator();
		while(it.hasNext())
		{
			Student stu = (Student)it.next();
			sop(stu.getName()+"::"+stu.getAge());
		}
	}
	public static void sop(Object obj)
	{
		System.out.println(obj);
	}
}
class Student implements Comparable //该接口强制让学生具备比较性
{
	private String name;
	private int age;
	Student(String name,int age)
	{
		this.name=name;
		this.age=age;
	}
	public int compareTo(Object obj)   //重写compareTo方法
	{
		if(!(obj instanceof Student))
			throw new RuntimeException("不是学生对象");
		Student s = (Student)obj;
		System.out.println(this.name+"...compare.."+s.name);
		if(this.age > s.age)
			return 1;
		if(this.age == s.age)
			return this.name.compareTo(s.name);
		return -1;
	}
	public String getName() 
	{
		return name;
	}
	public int getAge()
	{
		return age;
	}
}
class MyCompare implements Comparator  //定义比较器
{
	public int compare(Object o1,Object o2)   //重写compare方法
	{
		Student s1 = (Student)o1;
		Student s2 = (Student)o2;
		int num = s1.getName().compareTo(s2.getName());
		if(num == 0)
		{
			if(s1.getAge()>s2.getAge())
				return 1;
			if(s1.getAge()==s2.getAge())
				return 0;
			return -1;
		}
		return num;
	}
}
运行结果：
java01::11
java02::10
java04::14
java04::15
java08::14

```
## 泛型
### 泛型概述
- JDk1.5版本之后出现新特性。用于解决安全问题，是一个类型安全机制

### 使用泛型的好处
1. 将运行时期出现的问题ClassCastException(类型转换异常)，转移到了编译时期，方便程序员解决问题。
2. 避免了强制转换的麻烦。

### 泛型的格式
通过<>来接收类型的，当使用集合时，将集合中要存储的数据类型作为参数传递给<>中即可
### 泛型类
- 例如：
```
//泛型类
class Utils<T>
{
	private T t;
	public void setObject(T t)
	{
		this.t = t;
	}
	public T getObject()
	{
		return t;
	}
}
class Demo
{
    ...
    Utils<Worker> u = new Utils<Worker>();
	u.setObject(new Worker());
	Worker w = u.getObject();
}
```

### 泛型方法
- 例如：
```
package collection;
import java.util.*;
/**
 * 泛型方法
 * @author code-conqueror
 *
 */
public class GenericDemo {
	public static void main(String[] args)
	{
		Demo de = new Demo();
		de.print("HelloWorld");		//print: HelloWorld
		de.print(4);	//print: 4
	}
}

//泛型方法
class Demo
{
	public<Q> void print(Q q)
	{
		System.out.println("print: "+q);
	}
}
```

### 静态方法泛型
- **特别之处：**
    - 静态方法不可以访问类上定义的泛型。
    - 如果静态方法操作的应用数据类型不确定，可以将泛型定义在方法上</br>
- 例如：
```
package collection;
import java.util.*;
/**
 * 静态方法泛型
 * @author code-conqueror
 *
 */
public class GenericDemo {
	public static void main(String[] args)
	{
		Demo de = new Demo();
		de.print("happy!");		//print: happy!
	}
}

//泛型方法
class Demo
{
	public static <W> void print(W w)
	{
		System.out.println("print: "+w);
	}
}
```

### 泛型接口
- 例如
```
package collection;
import java.util.*;
/**
 * 泛型接口
 * @author code-conqueror
 *
 */
public class GenericDemo1 {
	public static void main(String[] args)
	{
		InterImp1 i1 = new InterImp1();
		i1.show("ad");	//show: ad
		//i1.show(4);	//编译出错
		InterImp2 i2 = new InterImp2();	
		i2.show("ad");	//show: ad
		i2.show(4);		//show: 4
	}
}

//泛型接口，格式一
interface Inter1<T>
{
	void show(T t);
}
class InterImp1 implements Inter1<String>
{
	public void show(String t)
	{
		System.out.println("show: "+t);
	}
}
//泛型接口，格式二
interface Inter2<T>
{
	void show(T t);
}
class InterImp2<T>  implements Inter2<T>
{
	public void show(T t)
	{
		System.out.println("show: "+t);
	}
}
```

### 泛型限定
1. **通配符：**? ，如<?>
2. **泛型的限定**</br>
    - <? extends E> ：可以接受E类型或 E的子类型。上限
    - <? super E>：可以接收E类型或者E的父类型。下限

### java的泛型与c++中模板的区别
1. C++模板可以使用基本数据类型比如int，但是java不行，必须要转为Integer
2. c++中类型的参数可以实例化，但是java不支持
3. java中类型参数不能用于静态变量和方法，以为会被共享，在c++中是可以的
4. 在java中不管参数类型是什么，其类的所有类型是同一类型。类型参数会在运行时被抹去，但是c++参类数类型不同，实例类型也不同。</br>

- [模板类与泛型类区别](https://blog.csdn.net/howard2005/article/details/79284411)

# 集合
## Map
### Map的概述
- 该集合存储键值对。一对一对往里存，而且要保证键的唯一性
- set底层就是使用Map集合

### Map的共性方法
1. **添加**</br>
`添加元素，如果添加时出现相同的键，那么后添加的值会覆盖原有键的值，并返回原有的值`
    - put(K key , V value)
    - putAll(Map<? extends K,? extends V> m)
2. **删除**
    - clear()
    - remove(Object key)
3. **判断**
    - containValue(Object value)
    - containsKey(Object key)
    - isEmpty()
4. **获取**</br>
- **Map集合的两种取出方式：**</br>
1. keySet：
将Map中所有的键存入到Set集合。因为Set具备迭代器。所有可以迭代方式取出的键，再根据get方法，获取每一个键对应的值。</br>
**示例：**
```
package collection;
import java.util.*;
/**
 * 
 * @author code-conqueror
 *
 */
public class MapDemo {
	public static void main(String[] args)
	{
		Map<String,String> map = new HashMap<String,String>();
		//添加元素
		map.put("02","zhangsan02");
		map.put("03","zhangsan03");
		map.put("01","zhangsan01");
		map.put("04","zhangsan04");
		//先获取Map集合所有键的Set集合
		Set<String> keySet = map.keySet();
		//有了Set集合，就可以获取迭代器
		Iterator<String> it = keySet.iterator();
		while(it.hasNext())
		{
			String key = it.next();
			//有了键就可以通过map集合的get方法获取其对应的值
			String value = map.get(key);
			System.out.println("key: "+key+",value: "+value);
		}
	}
}
运行结果：
key: 01,value: zhangsan01
key: 02,value: zhangsan02
key: 03,value: zhangsan03
key: 04,value: zhangsan04

```

2. Set<Map.Entry<k,v>> entrySet：将Map集合中的映射关系存入到Set集合中，而这个关系的数据类型就是：Map.Entry</br>
**示例：**
```
package collection;
import java.util.*;
/**
 * 
 * @author code-conqueror
 *
 */
public class MapDemo {
	public static void main(String[] args)
	{
		Map<String,String> map = new HashMap<String,String>();
		map.put("02","zhangsan02");
		map.put("03","zhangsan03");
		map.put("01","zhangsan01");
		map.put("04","zhangsan04");
		//将Map集合中的映射关系存入到Set集合中，而这个关系的数据类型就是：Map.Entry
		Set<Map.Entry<String, String>> entrySet = map.entrySet();
		Iterator<Map.Entry<String, String>> it = entrySet.iterator();
		while(it.hasNext())
		{
			Map.Entry<String, String> me =it.next();
			String key = me.getKey();
			String value = me.getValue();
			System.out.println(key+":"+value);
		}
 	}
}
运行结果：
key: 01,value: zhangsan01
key: 02,value: zhangsan02
key: 03,value: zhangsan03
key: 04,value: zhangsan04

```

### Hashtable
- **定义：** 底层是哈希表数据结构，不可以存入null键，该集合是线性同步的。JDk1.0出现，效率低

### HashMap
 - **定义：** 底层是哈希表数据结构，允许null值和null键，该集合是不同步的。JDK1.2出现，效率高

### TreeMap
- **定义：** 底层是二叉树数据结构。线程不同步。可以用于Map集合中的键进行排序

### Map扩展知识
- Map集合被使用是因为具备映射关系