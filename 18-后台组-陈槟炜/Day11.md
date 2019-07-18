[toc]
## StringBuffer
### 一、StringBuffer基础知识
#### 1. StringBuffer是字符串缓冲区,是一个容器
#### （一）特点
1. 长度是可以改变的
2. 可以直接操作多个数据类型
3. 最终会通过toString方法变成字符串

#### （二）常见功能
    C:create    U:upadte    R:read      D:delete
##### 1). **存储**  
1. **StringBuffer append():** 将指定的数据作为参数添加到已有数据结尾处  
2. **StringBuffer insert(index,数据):** 可以将数据插入到指定index位置
```
package package1;
/**
 * StringBuffer添加
 * @author code-conqueror
 *
 */
public class StringBufferDemo {
	public static void main(String[] args)
	{
		StringBuffer sb = new StringBuffer();
		StringBuffer sb1 = sb.append(34);
	1.	sop(sb.toString());			//34
		sop(sb1.toString());		//34
		sop(""+(sb==sb1));			//true
	2.	sb.append("abc").append("").append("dsda").append("sda");	//方法调用链
		sop(sb.toString()); //34abcdsdasda
	3.  sb.insert(1, "qq");
		sop(sb.toString()); //3qq4abcdsdasda
	}
	public static void sop(String str)
	{
		System.out.println(str);
	}
}

```
##### 2). **删除**
1. **StringBuffer delete(start,end):** 删除缓冲区中的数据，包括start,不包括end
2. **StringBuffer deleteCharAt(index):** 删除指定位置的字符

##### 3). **获取** 
1. **char c=stbf1.charAt(index);**
2. **int c=stbf1.indexOf(String);**
3. **int c=stbf1.lastIndexOf(string);**

##### 4). **修改**
1. **StringBuffer stbf=stbf1.replace(Start,end,string);**
2. **void setCharAt(index,char);**
