[toc]
# 集合
## Collections-sort
- Collections.sort用于排序
- 示例：
```
package task5;
import java.util.*;
public class Test1 {
	public static void main(String[] args)
	{
		List<String> list = new ArrayList<String>();
		list.add("sdsad");
		list.add("fgdsad");
		list.add("sdssdd");
		list.add("xcz");
		Collections.sort(list);     //排序
		Iterator<String> it = list.iterator();
		while(it.hasNext())
		{
			sop(it.next());
		}
	}
	public static void sop(Object obj)
	{
		System.out.println(obj);
	}
}
运行结果
fgdsad
sdsad
sdssdd
xcz

```

## Collections-max
- Collections.max 用于集合元素的最大值
- 示例：
```
package task5;
import java.util.*;
public class Test1 {
	public static void main(String[] args)
	{
		List<String> list = new ArrayList<String>();
		list.add("sdsad");
		list.add("fgdsad");
		list.add("sdssdd");
		list.add("xcz");
		sop(list);
		String max = Collections.max(list);
		sop("Max="+max);
		Collections.sort(list);
		Iterator<String> it = list.iterator();
		while(it.hasNext())
		{
			sop(it.next());
		}
	}
	public static void sop(Object obj)
	{
		System.out.println(obj);
	}
}

运行结果
[sdsad, fgdsad, sdssdd, xcz]
Max=xcz
fgdsad
sdsad
sdssdd
xcz

```

## Collections-binarySearch
- Collections.binarySearch：使用二分搜索法搜索指定列表，以获得指定对象，找到的话返回角标，找不到的话返回（-插入点-1）
- 前提：列表必须是有序的
- 示例
```
package task5;
import java.util.*;
public class Test1 {
	public static void main(String[] args)
	{
		List<String> list = new ArrayList<String>();
		list.add("sdsad");
		list.add("fgdsad");
		list.add("sdssdd");
		list.add("xcz");
		Collections.sort(list);
		sop(list);
		int index1 = Collections.binarySearch(list,"sdssdd");	//查找元素所在的位置
		sop(index1);
		int index2 = Collections.binarySearch(list,"abc");	//不存在返回（-插入点-1）
		sop(index2);
		
	}
	public static void sop(Object obj)
	{
		System.out.println(obj);
	}
}
运行结果：
[fgdsad, sdsad, sdssdd, xcz]
2
-1

```

## Collections-替换反转
- Collections.fill(集合对象名,元素t)：将集合中的元素全部替换成元素t
- Collections.replaceAll(集合对象名,元素a,元素b):将元素a替换为元素b
- Collections.reverse(集合对象名)：反转指定列表中元素的顺序

## Collections-reverseOrder
- Collections.reverseOrder：返回一个比较器，塔强行逆转指定比较器的顺序
- 示例：
```
package task5;
import java.util.*;
public class Test1 {
	public static void main(String[] args)
	{
		TreeSet<String> ts = new TreeSet<String>(Collections.reverseOrder());
		ts.add("sdsad");
		ts.add("fgdsad");
		ts.add("sdssdd");
		ts.add("xcz");
		sop(ts);    //[xcz, sdssdd, sdsad, fgdsad]

		
	}
	public static void sop(Object obj)
	{
		System.out.println(obj);
	}
}
```

## Collections-Synlist
- synchronizedList
    - public static <T> List<T> synchronizedList(List<T> list)
返回指定列表支持的同步（线程安全的）列表。为了保证按顺序访问，必须通过返回的列表完成 所有对底层实现列表的访问。
    - 在返回的列表上进行迭代时，用户必须手工在返回的列表上进行同步：
- 示例：
```
    List list = Collections.synchronizedList(new ArrayList());
        ...
    synchronized(list) {
        Iterator i = list.iterator(); // Must be in synchronized block
        while (i.hasNext())
            foo(i.next());
    }
```

## Arrays
- Arrays：用于操作数组的工具类。</br>**里面都是静态方法**
- asList:将数组变成list集合

### 把数组变成list集合的好处  
- 可以使用集合的思想和方法来操作数组中的元素
- 示例：
```
package collection;
import java.util.*;
public class ArrayDemo {
	public static void main(String[] args)
	{
		int[] arr = {1,3,5,7};
		System.out.println(Arrays.toString(arr));	//[1, 3, 5, 7]
		
		//将数组变成集合
		String[] arr1 = {"abc","cc","kkk"};
		List<String>list =Arrays.asList(arr1);	
		System.out.println(list);	//[abc, cc, kkk]
		Integer[] arr2 = {1,3,5,7};
		List<Integer> li1 = Arrays.asList(arr2);	//[1, 3, 5, 7]
		System.out.println(li1);
		
	}
}

```
- **注意**
    - 将数组变成集合，不可以使用集合的增删方法，因为数组的长度是固定的，如果增删会发生异常
    - 如果数组中的元素都是对象。那么变成集合时，数组中的元素就直接转成集合中的元素。
    - 如果数组中的元素都是基本数据类型，那么会将该数组作为集合中的元素存在
- 示例：
```
package collection;
import java.util.*;
public class ArrayDemo {
	public static void main(String[] args)
	{
		//如果数组中的元素都是基本数据类型，那么会将该数组作为集合中的元素存在
		int[] arr = {1,3,5,7};
		List<int[]> li = Arrays.asList(arr);	//[[I@279f2327]
		System.out.println(li);
		
		//如果数组中的元素都是对象。那么变成集合时，数组中的元素就直接转成集合中的元素。
		String[] arr1 = {"abc","cc","kkk"};
		List<String>list =Arrays.asList(arr1);	
		System.out.println(list);				//[abc, cc, kkk]
		
		Integer[] arr2 = {1,3,5,7};
		List<Integer> li1 = Arrays.asList(arr2);	//[1, 3, 5, 7]
		System.out.println(li1);
		
	}
}

```

## 集合转成数组
- 用Collection接口的toArray方法
- 指定类型的数组的长度的定义
    1. 当指定类型的数组长度小于集合的size，那么该方法内部就会创建一个新的数组。长度为集合的size。</br>
    2. 当指定类型的数组长度大于集合的size，就不会新创建数组，而是使用传递进来的数组。
- 为什么要将集合变数组？</br>
    为了限定对元素的操作。不需要增删。
- 示例：
```
package collection;
import java.util.*;
public class ToArrayDemo {
	public static void main(String[] args)
	{
		List<String> li = new ArrayList<String>();
		li.add("java01");
		li.add("java02");
		li.add("java03");
		li.add("java04");
		String[] arr = li.toArray(new String[li.size()]);
		for(String i: arr)
			System.out.println(i);
	}
}
运行结果：
java01
java02
java03
java04

```

## 增强for循环
- **格式：**</br>
```
for(数据类型 变量名：被遍历的集合(Collection)或者数组)
{
    
}
```
- 对集合进行遍历，只能获取集合元素，但是不能对集合进行操作
- 迭代器除了遍历，还可以进行remove集合中元素的动作。如果用LisIterator,还可以在遍历的过程中可以对集合进行增删改查操作。

### 传统for循环和高级for循环的区别
- 高级for有一个局限性，必须有被遍历的目标
- 建议在遍历数组的时候，希望用传统for。因为传统for可以定义角标

## 可变参数
- JDK1.5版本后出现的新特性。就是数组参数的简写形式。不用每一次都手动的建立数组对象只要将要操作的元素作为参数传递即可。虚拟机会隐式将这些参数封装为数组
- 示例：
```
package collection;

public class ArrayDemo1 {
	public static void main(String[] args)
	{
		show(1,2,3,4);		//1 2 3 4
		show(1,2);			//1 2
		show(0);			//0
	}
		
	public static void show(int... arr)		//"..." 为可变参数的形式
	{
		for(int i:arr)
		{
			System.out.print(i+" ");		
		}
		System.out.println();
	}
}

```

## 静态导入
- 示例

> import static java.lang.System.*;   　//导入System类中的所有静态成员。
