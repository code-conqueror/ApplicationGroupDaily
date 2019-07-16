### 一、包(package)
1. **定义:** </br>    
    - 对文件进行分层管理，</br>
    - 给类提供多层面命名空间</br>
    - `类的全名为：包名.类名`
    - 包也是一种封装形式</br>

2. **规范:** 定义包时，所有字母小写
3. **命令提示符下的使用形式**</br>
    javac -d 包所在的目录 类名.java
4. **总结**
    - 包与包之间进行访问，被访问的包中的类以及类的成员，需要public修饰
    - 不同包中的之类还可以直接访问父类中被protected权限修饰的成员
5. 包与包之年间可以使用的权限只有两种：`public`、`protected`
6. **权限从大到小为**</br>
    public  >  protected  >  default (默认) > peivate
### 二、import语句
1. **定义：**</br>
import可以导入包中的类和接口</br>
2. **使用格式如:**
```
    C:\mayclass\packb\DemoA.class
    C:\mayclass\packb\DemoB.class
    C:\mayclass\packb\packb_a\DemoZ.class
    
    1. import packb.DemoA;   //导入DemoA类
    2. import packb.packb_a.DemoZ   //导入DemoZ类
    3. import packb.*   //把packb中所有的类都导入，包括DemoA、DemoB,不包括包pacb_a
```
- 建议不使用`通配符（*）
- 建议包名不要重复，可以使用utl来定义，url是唯一的
### 三、多线程
1. **进程：** 是一个正在执行中的程序。</br>
  每个进程执行都有一个执行顺序，该顺序是一个执行路径
2. **线程：** 就是进程中的一个独立的控制单元
3. 一个进程至少有一个线程
4. 创建线程得第一种方式：继承Thread类</br>
    步骤：
    1. 定义类继承Thread
    2. 复写Thread类中的run方法
    3. 调用线程的start方法</br>该方法有两大作用：启动线程，调用run方法
        