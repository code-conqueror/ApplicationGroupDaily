### 函数
- 函数是具有特定功能的独立小程序
- 函数也叫方法
- 函数中只能调用函数，不能定义函数
- 函数的形式：  
```
修饰符（如public static) 返回类型 函数名（参数列表）
{
    ...
    return ...;
}
```  
- 函数的定义:
    - 明确函数功能的结果
    - 有没有未知的内容参与运算
- **定义函数时只需要完成相对应的功能，不要做过多的功能**  

#### 函数重载
- 当定义的功能相同，但参数列表不同，这时需要定义一个函数名称表示去功能，通过参数列表的不同来区分同名函数
- 重载和返回值类型无关  

### 数组
- 同一种类型的集合，数组就是容器,数组类型。
```
格式一：
    元素类型[]  数组名 = new 元素类型[元素个数或数组长度];   
    如：int[]  str = new int[5];
格式二：
    元素类型[]  数组名 = new 元素类型[]{元素,元素,...};
    如：int[]  str = new int[]{1,2,3}; 
        int[]  str = {1,2,3};
```
- 栈内存：数据使用完会**自动释放**掉
    - 凡是局部变量都在栈内存里
- 堆内存：new出来的实体都在对堆里面开辟一个空间，实体包括数组和对象
    - **内存地址值**：堆内存里的每个实体都有一个存放位置（地址）
    - **默认初始化值**：堆内存的实体都有封装数据，都有默认值
    - **垃圾回收机制**：当对象和实体在堆内存变成垃圾，java虚拟就会自动启动垃圾回收机制不定时将堆内存中不再使用的内存清理掉。
        - 如int[] x=new int[5];
        - x=null;
        - 此时new int[5]就会带堆内存中变成垃圾,被不定时清除掉
#### 数组使用
- 求数组长度：数组名称.length
- 数组名为地址，如：[I@...  数组，整型，...哈希值表示
- 折半查找法条件：有序的数组
```
public static int halfSearch(int[] arr,int key)
	{
		int min=0;
		int max=arr.length;
		int mid=(min+max)/2;
		while(key!=arr[mid])
		{
			if(key>arr[mid])
				min=mid+1;
			else if (key<arr[mid])
				max=mid-1;
			if(min>max)
				return -1;
			mid=(max+min)>>1;
		}
		return mid;
	}
```
- StringBuffer能装数据的容器，可以反转打印数据
    > StringButter sb=new StringBuffer();
    > System.out.println(sb.reserve);
```
package project1;

public class Test9 {
	public static void main(String[] args)
	{
		//toBin(6);
		toHex(60);
	}
	/*十进制->十六进制*/
	public static void toHex(int num)
	{
		StringBuffer sb=new StringBuffer();
		for(int x=0;x<8;x++)
		{
			int temp=num&15;
			if(temp>9)
				//System.out.println((char)(temp-10+'A'));
				sb.append((char)(temp-10+'A'));
			else
				//System.out.println(temp);
				sb.append(temp);
			num=num>>>4;
		}
		System.out.println(sb.reverse());
	}
	/*十进制->二进制*/局限性：只能算正数
	public static void toBin(int num)
	{
		StringBuffer sb=new StringBuffer();
		while(num>0)
		{
			sb.append(num%2);
			num/=2;
		}
		System.out.println(sb.reverse());
	}
	public static void toBin(int num)
	{
		char[] chs = {'0','1'};			//列表法,定义二进制的表
		char[] arr = new char[32];
		int pos = arr.length;
		while(num!=0)
		{
			int temp = num & 1;
			arr[--pos] = chs[temp];
			num=num>>>1;
		}
		for(int x=pos;x<arr.length;x++)
		{
			System.out.print(arr[x]);
		}
		System.out.println();
	}
}
}

```
### 二维数组
- 格式：
    > int [][] arr=new int [3][2];  
    > int [][] arr=new int [3][];
	