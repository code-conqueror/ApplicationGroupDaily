[toc]
# GUI
## GUI概述
**一、GUI和CLI**
- GUI
    - Graphical User Interface(图形用户接口)
    - 用图形的方式来显示计算机操作界面，这样更方便更直观
- CLI
    - Command line User Interface(命令行用户接口)
    - 就是常见的Dos命令行操作
    - 缺点：操作不直观
- Java为GUI提供的对象都存在java.Awt和javax.Swing两个包中</br>

**二、Awt和Swing**
- java.Awt：Abstract Window ToolKit(抽象窗口工具包)，需要调用本地系统方法实现功能。属于重量级控件
- javax.Swing：在Awt的基础上，建立的一套图形界面系统，其中提供了更多的组件，而且完全有Java实现，增强了移植性，属轻量级控件。
**三、继承关系图**
```
graph TB
A(Component构架)-->B(Container容器)
A-->C(Button按钮)
A-->D(TextComponent文本组件)
D-->D1(TextArea文本区域)
D-->D2(TextField文本框)
A-->E(Label标签)
A-->F(Checkbox复选框)
B-->B1(Window窗口)
B-->B2(Panel面板)
B1-->C1(Frane框架)
B1-->C2(Dialog对话框)
C2-->H(FileDialog文件对话框)
```
```
Container：为容器，是一个特殊的组件，该组件中可以通过add方法添加其他组建进来
```

## 布局
**一、 定义：**</br>
容器中的组件的排放方式</br>
**二、常见的布局管理器**
- **FlowLayout** (流式布局管理器)
    - 居中，从左向右排序
    - Panel的布局管理器
- **BorderLayout**(边界布局管理器)
    - 东南西北中
    - Frame默认管理器
- **GridLayout**(网格布局管理器)
    - 网格，规则的矩阵
- **GirdBagLayout**(网格包布局管理器)
    - 非规则的矩阵
- **GardLayout**(卡片布局管理器)
    - 选项卡片

## Frame
**一、创建图形化界面**
1. 创建Frame窗体
2. 对窗体进行基本设置
3. 定义组件
4. 将组件通过窗体的add方法添加到窗体中
5. 让窗体显示，通过setVeriable(true)</br>

**二、常用方法**
- **Frame(String title)**</br>
    title 为标题
- **void setVisible(boolean b)**</br>
    根据b的值显示或隐藏组件
- **void setSize(length,wide)**</br>
    设置窗口的长宽
- **void setLocation(x,y)**</br>  设置宽口的位置(x,y)为窗口左上一点与屏幕的距离
- **void setbounds(x,y,length,wide)**

**三、示例**
```
package Container;
import java.awt.*;
public class FrameDemo 
{
	public static void main(String[] args) 
	{
		Frame f = new Frame("my awt");	//"my awt"是标题
		f.setSize(500,400);
		Button b = new Button("按钮");
		f.add(b);
		f.setVisible(true);	//显示隐藏组件
	}

}

```
## 事件监听机制
**一、事件监听机制的特点**</br>
1. **事件源**</br>
    就是awt包或者swing包中的那些图形界面组件
2. **事件**</br>
    每一个事件源都有自己特有的对应时间和共性事件
3. **监听器**</br>
    将可以触发某一个事件的动作封装到监听器中
4. **事件处理**

**二、 [事件监听机制流程图](https://image.baidu.com/search/detail?ct=503316480&z=0&ipn=d&word=%E4%BA%8B%E4%BB%B6%E7%9B%91%E5%90%AC%E6%9C%BA%E5%88%B6%E6%B5%81%E7%A8%8B%E5%9B%BE&step_word=&hs=2&pn=0&spn=0&di=110&pi=0&rn=1&tn=baiduimagedetail&is=0%2C0&istype=0&ie=utf-8&oe=utf-8&in=&cl=2&lm=-1&st=undefined&cs=410962832%2C3416917299&os=4050972115%2C2043786956&simid=0%2C0&adpicid=0&lpn=0&ln=721&fr=&fmq=1564109563517_R&fm=&ic=undefined&s=undefined&hd=undefined&latest=undefined&copyright=undefined&se=&sme=&tab=0&width=undefined&height=undefined&face=undefined&ist=&jit=&cg=&bdtype=0&oriquery=&objurl=http%3A%2F%2Fs7.sinaimg.cn%2Fmw690%2F614f347bgdd6fa76c7036%26690&fromurl=ippr_z2C%24qAzdH3FAzdH3Fks52_z%26e3Bftgw_z%26e3Bv54_z%26e3BvgAzdH3FfAzdH3Fks52_v0w9bubma8adon9a_z%26e3Bip4s&gsm=0&rpstart=0&rpnum=0&islist=&querylist=&force=undefined)**

## 窗体事件
**一、常用方法**

1. **public void addWindowListener(WindowListener l)**</br>
添加指定的窗口侦听器，以从此窗口接收窗口事件。如果 l 为 null，则不抛出任何异常，且不执行任何操作。
2. **windowClosing(WindowEvent e)</br>{System.exit(0);}** </br>
    关闭窗口
3. **windowActivated(WindowEvent e)**</br>   窗口前置
4. **windowOpened(WindowEvent e)**</br>   窗口打开
    

**二、示例**
```
package Container;
import java.awt.*;
import java.awt.event.*;
public class FrameDemo 
{
	public static void main(String[] args) 
	{
		Frame f = new Frame("my awt");	//"my awt"是标题
		f.setSize(500,400);
		f.setLocation(300,200);
		f.setLayout(new FlowLayout());
		
		Button b = new Button("按钮");
		f.add(b);
		f.addWindowListener(new WindowAdapter()
				{
					public void windowClosing(WindowEvent e)
					{
						System.out.println("关闭窗口");
						System.exit(0);
					}
					public void windowActivated(WindowEvent e)
					{
						System.out.println("窗口前置");
					}
					public void windowOpened(WindowEvent e)
					{
						System.out.println("窗口打开");
					}
				});
		f.setVisible(true);	//显示隐藏组件
	}

}

```
## Action事件
**示例**
```
package Container;
import java.awt.*;
import java.awt.event.*;
public class FrameDemo2 
{
	private Frame f;
	private Button but;
	FrameDemo2()
	{
		init();
	}
	public void init()
	{
		f = new Frame("my frame");
		f.setBounds(300, 100, 500, 400);
		f.setLayout(new FlowLayout());
		but = new Button("my button");
		f.add(but);
		
		myEvent();
		
		f.setVisible(true);
	}
	private void myEvent()
	{
		f.addWindowListener(new WindowAdapter()
				{
					public void windowClosing(WindowEvent e)
					{
						System.exit(0);
					}
				});
		//让按钮具备退出的功能，选择哪个监听器？
		but.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e)
					{
						System.out.println("按钮退出");
						System.exit(0);
					}
				});
		
	}
	public static void main(String[] args) 
	{
		new FrameDemo2();
	}
	
}

```

## 鼠标事件
**示例**
```
package Container;
import java.awt.*;
import java.awt.event.*;
public class MouseAndKeyEventDemo
{
	private Frame f;
	private Button but;
	MouseAndKeyEventDemo()
	{
		init();
	}
	public void init()
	{
		f = new Frame("my frame");
		f.setBounds(300, 100, 500, 400);
		f.setLayout(new FlowLayout());
		but = new Button("my button");
		f.add(but);
		
		myEvent();
		
		f.setVisible(true);
	}
	private void myEvent()
	{
		f.addWindowListener(new WindowAdapter()
				{
					public void windowClosing(WindowEvent e)
					{
						System.exit(0);
					}
					
				});
		//让按钮具备退出的功能，选择哪个监听器？
		but.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e)
					{
						System.out.println("action ok");
					}
				});
		//添加鼠标监听
		but.addMouseListener(new MouseAdapter()
				{
					private int count = 1;
					private int clickCount = 1;
					public void mouseEntered(MouseEvent e)
					{
						System.out.println("鼠标进入该组件"+count++);
					}
					public void mouseClicked(MouseEvent e)				//双击鼠标
					{
						if(e.getClickCount()==2)
							System.out.println("双击按钮"+clickCount++);
					}
				});
		
		
	}
	public static void main(String[] args) 
	{
		new MouseAndKeyEventDemo();
	}
	
}

```
## 键盘事件
**一、常见应用**

- 在but上按键盘就出现相应文字和对应的编码
```
Button but = new Button("my button");
//添加键盘监听
but.addKeyListener(new KeyAdapter()        //KeyAdapte()-->键盘监听器
				{
					public void keyPressed(KeyEvent e)
					{
						System.out.println(e.getKeyText(e.getKeyCode())+"..."+e.getKeyCode());
					}
				});
```

- 在but上，用键盘按回车就退出
```
Button but = new Button("my button");
but.addKeyListener(new KeyAdapter()
				{
					public void keyPressed(KeyEvent e)
					{
					    //getKeyCode()获得其编码值，KeyEvent.VK_ENTER为选定的常量字段值
						if(e.getKeyCode()==KeyEvent.VK_ENTER)   
							System.exit(0);
					}
				});
```
- 在but上，用键盘同时按Ctrl和回车键退出
```
Button but = new Button("my button");
but.addKeyListener(new KeyAdapter()
				{
					public void keyPressed(KeyEvent e)
					{
						if(e.isControlDown()&&e.getKeyCode()==KeyEvent.VK_ENTER)   
							System.exit(0);
					}
				});
```
- 为文本框内只能输入0~9，其余非法
```
    TextField tf = new TextField(20);
		tf.addKeyListener(new KeyAdapter()
				{
					//文本框只能输入0~9，非法字符录不进去
					public void keyPressed(KeyEvent e)
					{
						int code = e.getKeyCode();
						if(!(code>=KeyEvent.VK_0&&code<=KeyEvent.VK_9))
						{
							System.out.println(code+"....是非法的");
							e.consume();	//非法字符录不进去
						}
					}
				});
```

**二、典例**
```
package Container;
import java.awt.*;
import java.awt.event.*;
public class MouseAndKeyEventDemo2
{
	private Frame f;
	private Button but;
	private TextField tf;		//添加文本框变量
	MouseAndKeyEventDemo2()
	{
		init();
	}
	public void init()
	{
		f = new Frame("my frame");
		f.setBounds(300, 100, 500, 400);
		f.setLayout(new FlowLayout());
		but = new Button("my button");
		tf = new TextField(20);
		f.add(tf);		//窗体添加文本框
		f.add(but);
		
		myEvent();
		
		f.setVisible(true);
	}
	private void myEvent()
	{	
		//按红x退出
		f.addWindowListener(new WindowAdapter()
				{
					public void windowClosing(WindowEvent e)
					{
						System.exit(0);
					}
				});
		//给but添加键盘监听
		but.addKeyListener(new KeyAdapter()
				{
					public void keyPressed(KeyEvent e)
					{	
						if(e.isControlDown()&&e.getKeyCode()==KeyEvent.VK_ENTER)
							System.exit(0);
					}
				});
		tf.addKeyListener(new KeyAdapter()
				{
					//文本框只能输入0~9，非法字符录不进去
					public void keyPressed(KeyEvent e)
					{
						int code = e.getKeyCode();
						if(!(code>=KeyEvent.VK_0&&code<=KeyEvent.VK_9))
						{
							System.out.println(code+"....是非法的");
							e.consume();	//非法字符录不进去
						}
					}
				});
	}
	public static void main(String[] args) 
	{
		new MouseAndKeyEventDemo2();
	}
}

```

### 示例（列出指定目录内容）
```
package Container;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
/**
 * 
 * 自己没看视频后写的，感觉自信心爆棚
 * @author DELL
 *
 */
public class FrameDemo1
{
	private Frame f;
	private Button but;
	private TextField tf;
	private TextArea ta;
	
	FrameDemo1()
	{
		init();
	}
	public void init()
	{
		f = new Frame("my frame");
		but = new Button("转到");
		tf = new TextField(30);
		ta = new TextArea(15,40);	//设置文本区域,20为行数，50为列数
		f.setLayout(new FlowLayout());	//添加布局管理器
		f.setBounds(400, 200, 400, 325);
		f.add(tf);
		f.add(but);
		f.add(ta);
		event();
		f.setVisible(true);
	}
	public void event()
	{
		f.addWindowListener(new WindowAdapter()
		{
			public void windowClosing(WindowEvent e)
			{
				System.exit(0);
			}
		});
		but.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e)
					{
//						String text = tf.getText();	//将文本框里的文字保存在text中
//						ta.setText(text);		//将text转到ta中
//						//System.out.println(text);
//						tf.setText("");
						String dirPath = tf.getText();
						File dir = new File(dirPath);
						if(dir.exists()&&dir.isDirectory())
						{
							ta.setText("");
							String[] names = dir.list();
							for(String name: names)
							{
								ta.append(name+"\r\n");
							}
						}
						
						
					}
				});
	}
	public static void main(String[] args) 
	{
		new FrameDemo1();
	}
	
}

```
## 对话框
- Dialog
- **示例**
```
package Container;
import java.awt.*;
import java.awt.event.*;
import java.io.*;

public class FrameDemo1
{
	private Frame f;
	private Button but;
	private TextField tf;
	private TextArea ta;
	private Label lab; 
	private  Button okbut;
	private Dialog d;
	
	FrameDemo1()
	{
		init();
	}
	public void init()
	{
		f = new Frame("my frame");
		but = new Button("转到");
		tf = new TextField(30);
		ta = new TextArea(15,40);	//设置文本区域,20为行数，50为列数
		f.setLayout(new FlowLayout());	//添加布局管理器
		f.setBounds(400, 200, 400, 325);
		
		d = new Dialog(f,"提示信息-self",true);		//对话框
		d.setBounds(400, 200, 240, 150);
		d.setLayout(new FlowLayout());
		lab = new Label();
		okbut = new Button("确定");
		
		d.add(lab);
		d.add(okbut);
		
		f.add(tf);
		f.add(but);
		f.add(ta);
		event();
		f.setVisible(true);
	}
	public void event()
	{
		okbut.addActionListener(new ActionListener()
		{
			public void actionPerformed(ActionEvent e)
			{
				d.setVisible(false);
			}
		});
			
		d.addWindowListener(new WindowAdapter()
		{
			public void windowClosing(WindowEvent e)
			{
				d.setVisible(false);
			}
			
		});
		f.addWindowListener(new WindowAdapter()
		{
			public void windowClosing(WindowEvent e)
			{
				System.exit(0);
			}
		});
		tf.addKeyListener(new KeyAdapter()
				{
					public void keyPressed(KeyEvent e)
					{
						if(e.getKeyCode()==KeyEvent.VK_ENTER)
							showdir();
					}
				});
		but.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e)
					{
						showdir();
					}
				});
	}
	private void showdir()
	{
		String dirPath = tf.getText();
		File dir = new File(dirPath);
		if(dir.exists()&&dir.isDirectory())
		{
			ta.setText("");
			String[] names = dir.list();
			for(String name: names)
			{
				ta.append(name+"\r\n");
			}
		}
		else
		{
			String info = "你输入有误，请重新输入";
			lab.setText(info);
			d.setVisible(true);
		}
	}
	public static void main(String[] args) 
	{
		new FrameDemo1();
	}
	
}

```

## 菜单MerBar
**示例**
```
package Container;
import java.awt.*;
import java.awt.event.*;
public class MyMenuDemo 
{
	private Frame f;
	private MenuBar mb;
	private Menu m,subMenu;
	private MenuItem closeItem,subItem;
	MyMenuDemo()
	{
		init();
	}
	public void init()
	{
		f = new Frame("my window");
		f.setBounds(300, 100, 500, 600);
		f.setLayout(new FlowLayout());
		mb = new MenuBar();
		m = new Menu("文件");
		subMenu = new Menu("子菜单");
		closeItem = new MenuItem("退出");
		subItem = new MenuItem("子条目");
		
		subMenu.add(subItem);
		m.add(subMenu);
		
		m.add(closeItem);
		mb.add(m);
		f.setMenuBar(mb);
		myEvent();
		f.setVisible(true);
	}
	private void myEvent()
	{
		f.addWindowListener(new WindowAdapter()
		{
			public void windowClosing(WindowEvent e)
			{
				System.exit(0);
			}
		});
		closeItem.addActionListener(new ActionListener() 
		{
			public void actionPerformed(ActionEvent e)
			{
				System.exit(0);
			}
		});
	}
	public static void main(String[] args)
	{
		new MyMenuDemo();
	}
}

```