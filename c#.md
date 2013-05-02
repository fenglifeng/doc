线性：

c#中的数组必须先new一下，分配具体的空间；

c#中画图可以用Graphics这个类，可以用以下代码获取消息句柄
	
	Graphics graphics = this.CreateGraphics();

可以用以下代码画图

	graphics.DrawLine()

矩形的话

	graphics.DrawRectangle()

椭圆的话

	graphics.DrawEllopse()

扇形

	graphics.DrawPie

贝赛尔曲线

	graphics.DrawBezier()

多边形 

	graphics.DrawPolygon()

文本信息

	graphics.DrawString()

##
-----------
chart控件：

修改X、Y轴的左边间隔之类的都像chart.ChartAreas[0].AxisX.Minimum之类的，其中[0]为成员编号

series设置图标的类型，比如是2d还是3d，是折线还是条形之类的。其中name可以修改，可以表示其颜色的数据，color可以设置其折线的颜色（假设是折线的话）；可以添加不同成员，用来表示不同数据。如果要添加数据节点最简单的方法是Chart1.Series[0].Points.AddXY(x,y);Chart1.Series[0].Points.RemoveAt(0);删除第0个数据

在X.Designer.cs中有各种资源的初始化，所谓的构造函数就是如此

##
-------------
新窗口载入

	this.Visible = false;
	Form2 picture = new Form2();
	if (picture.ShowDialog() == DialogResult.Cancel)
	{

		this.Visible = true;
	}
	// this.Visible = true; //这里木有用的，因为很快就执行下来了

以上代码是新窗口载入的一种常见的方式，也就是其实是new一个新的窗口出来即可，然后show一下，在原窗口中还可以对新窗口的返回做判断，以便做后续的判断

##
多线程

C＃中的多线程是很简单的。


	Thread  mythread;
	threadd = new Thread(new ThreadStart(sayhello));
	mythread.Start();

上面代码是初始化一个线程，并启动线程。

委托：如果在新的线程用，要调用主线程中的资源，这时候需要委托功能，委托的关键字是delegate.

	public delegate void SayhelloHander();  //委托声明
	private void sayhello()
	{

		//Console.WriteLine("hello world"); 	
		// Console.ReadLine();
		if (textBox1.InvokeRequired == false) //判断是否是在资源的主进程中
		{
			textBox1.Text = "hello world";
		}
		else
		{
			SayhelloHander say = new SayhelloHander(sayhello);  //委托实例化
			textBox1.Invoke(say); //委托调用
		}
	}

上面的代码中看到了委托的大体味道，先委托一个声明，其关键字delegate说明这是一个委托，再将委托实例化，最后通过资源的invoke来调用委托函数

