# 学习日记  

## 投影  

> (20190312,0313)  

1. UTM和高斯-克吕格投影  

1.1 概念

- UTM:
  > UTM(Universal Transverse Mercator projection), 是通用横轴墨卡托投影, 是一种等角横轴割圆柱投影. 椭圆柱割地球于南纬80度,北纬84度两条等高圈。投影后两条相割的经线上没有变形, 而中央经线的长度比为0.9996. 国际大地测量学会曾建议, 中央子午线投影后,其投影长度适当缩短, (即长度比例因子K为0.9996, 中央经线比例因子取0.9996是为了保证离中央经线约330K处有两条不失真的标准经线) 以减少投影边缘地区的长度变形. 这个建议就是统一横轴墨卡托投影, 也称为通用横轴墨卡投影, 简称为UTM投影.  

- 高斯-克吕格投影:  
  > 高斯—克吕格投影 (Gauss--Kruger), 是一种等角横切椭圆柱投影.  

1.2 两者之间的异同  

- 两者都是横轴墨卡托投影的变种.  

- 从投影的几何来看, 高斯-克吕格投影是等角横切椭圆柱投影, 投影后中央经线保持不变, 比例系数为1. UTM投影是等角横轴割圆柱投影, 圆柱割地球于南纬80度, 北纬84度两条等高圈. 投影后两条割线上没有长度变形, 中央经线上长度比为0.9996; 我国的卫星影像资料多采用UTM投影.  

- 从计算的结果来看, 两者主要差别在比例系数上, 高斯-克吕格投影的中央经线上的比例系数为1, UTM投影的为0.9996. 两个投影之间可近似的采用X(UTM)=0.9996*(高斯)和y(UTM)=0.9996*(高斯)（如果坐标纵轴Y值西移了500公里，转换时必顺将Y值减去500公里乘上比例系数后再加上500公里。）进行转换。为了保证精度可采用控制点上的比例因子K来代替0.9996.  

- 两个投影的投影正反解公式不同：

- 分带的方式不同：两个投影的分带起点不同，高斯-克吕格投影自首子午线起每隔经度6度自西向东分带。第一带的中央经度为3度。UTM投影自西经180度起，每隔经度6度自西向东分带，第一带的中央经度为177度。因此，高斯投影的第一带是UTM投影的第31带。两个投影都是按照分带方法各自进行投影，故各带坐标成独立系统。以中央经线投影为纵轴，赤道投影为横轴，两轴交点即为各带的坐标原点，为了避免出现负值，高斯-克吕格投影与UTM北半球投影中规定将坐标纵轴西移500公里后当做起始轴。而UTM南半球投影除了将纵轴西移500公里外，还将横轴南移了10000公里。为了区别每一个坐标系统属于哪一带，通常再横轴做标签加上带号。

## Qt  

> (20190314)

1. GUI中的mouseevent中的button()和buttons()

- button()  
  > Qt::MouseButton QMouseEvent::button() const  
Returns the button that caused the event.  
Note that the returned value is always Qt::NoButton for mouse move events.  
**翻译**: button()返回的是产生鼠标事件的按钮鼠标move事件总是返回Qt::NoButton.  

- buttons()  
  > Returns the button state when the event was generated. The button state is a combination of Qt::LeftButton, Qt::RightButton, Qt::MidButton using the OR operator. For mouse move events, this is all buttons that are pressed down. For mouse press and double click events this includes the button that caused the event. For mouse release events this excludes the button that caused the event.  
**翻译**: 返回的是产生事件的按钮状态。按钮状态时Qt::LeftButton, Qt::RightButton, Qt::MidButton或其运算组合。对于鼠标move事件,函数返回当前按下的所有按钮。对于鼠标按下或双击事件,返回导致事件发生的按钮。对于鼠标释放事件,不包含导致事件发生的按钮。  

- 示例  
  > 假设鼠标左键已经按下,如果移动鼠标,会发生的move事件,button()返回Qt::NoButton, 而buttons()返回LeftButton。  
再次按下了右键,会发生press事件,button()返回Qt::RightButton, 而buttons()返回Qt::LeftButton|Qt::RightButton。  
如果再移动鼠标,会发生move事件,button()返回Qt::NoButton, 而buttons()返回Qt::LeftButton|Qt::RightButton。
再松开左键,会发生Release事件,button()返回Qt::LeftButton, 而buttons()返回Qt::RightButton。
也就是说,button()返回"哪个按钮发生了此事件",buttons()返回"发生事件时哪些按钮还处于按下的状态"。  

## 编码问题(UTF-8)  

> 20190315  

1. 只有windows机器,所以只测试了在winsows下的uft8的问题.  

- 问题现象:  

    在winsows下,使用qt进行编程,出现`C4819:该文件包含不能再当前代码页(936)中表示的字符。请将该文件保存成Unicode格式以防止数据丢失`的问题。  
- 解决办法:  

    qt设置选项: **工具/选项/文本编辑器/行为/文件编码**;此处有两个选项,一个是编码格式,默认为utf8;另一个是uft8 BOM的处理办法,此处有三种处理办法,一种是"总是删除",一种是"如果编码是utf-8则添加",一种是目前存在了则保留。出现警告"C4819"的原因是uft8格式中没有BOM,也就是选项"总是删除"导致的问题,所以将**总是删除**,改成**如果编码是uft-8则添加**,再重新保存一下(最好是添加文字,再删除问题,使qt检测到该文件改动,再保存;如果没有检测到改动,单击保存按钮或Ctrl+S是不会重新保存的。),此后就不会再出现警告"C4819",再次提醒,仅仅是在**windows**下测试,其他平台没有测试,可能不适用。