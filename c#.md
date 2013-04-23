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

＃＃
chart控件：

修改X、Y轴的左边间隔之类的都像chart.ChartAreas[0].AxisX.Minimum之类的，其中[0]为成员编号

series设置图标的类型，比如是2d还是3d，是折线还是条形之类的。其中name可以修改，可以表示其颜色的数据，color可以设置其折线的颜色（假设是折线的话）；可以添加不同成员，用来表示不同数据。如果要添加数据节点最简单的方法是Chart1.Series[0].Points.AddXY(x,y);Chart1.Series[0].Points.RemoveAt(0);删除第0个数据


在X.Designer.cs中有各种资源的初始化，所谓的构造函数就是如此
