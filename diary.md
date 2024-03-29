﻿# 学习日记  

## 投影(UTM和高斯-克吕格投影)  

> (20190312,0313)  

1. 概念

    - UTM:
     > UTM(Universal Transverse Mercator projection), 是通用横轴墨卡托投影, 是一种等角横轴割圆柱投影. 椭圆柱割地球于南纬80度,北纬84度两条等高圈。投影后两条相割的经线上没有变形, 而中央经线的长度比为0.9996. 国际大地测量学会曾建议, 中央子午线投影后,其投影长度适当缩短, (即长度比例因子K为0.9996, 中央经线比例因子取0.9996是为了保证离中央经线约330K处有两条不失真的标准经线) 以减少投影边缘地区的长度变形. 这个建议就是统一横轴墨卡托投影, 也称为通用横轴墨卡投影, 简称为UTM投影.  

    - 高斯-克吕格投影:  
    > 高斯—克吕格投影 (Gauss--Kruger), 是一种等角横切椭圆柱投影.  

2. 两者之间的异同

- 两者都是横轴墨卡托投影的变种.  

- 从投影的几何来看, 高斯-克吕格投影是等角横切椭圆柱投影, 投影后中央经线保持不变, 比例系数为1. UTM投影是等角横轴割圆柱投影, 圆柱割地球于南纬80度, 北纬84度两条等高圈. 投影后两条割线上没有长度变形, 中央经线上长度比为0.9996; 我国的卫星影像资料多采用UTM投影.  

- 从计算的结果来看, 两者主要差别在比例系数上, 高斯-克吕格投影的中央经线上的比例系数为1, UTM投影的为0.9996. 两个投影之间可近似的采用X(UTM)=0.9996*(高斯)和y(UTM)=0.9996*(高斯)（如果坐标纵轴Y值西移了500公里，转换时必顺将Y值减去500公里乘上比例系数后再加上500公里。）进行转换。为了保证精度可采用控制点上的比例因子K来代替0.9996.  

- 两个投影的投影正反解公式不同：

- 分带的方式不同：两个投影的分带起点不同，高斯-克吕格投影自首子午线起每隔经度6度自西向东分带。第一带的中央经度为3度。UTM投影自西经180度起，每隔经度6度自西向东分带，第一带的中央经度为177度。因此，高斯投影的第一带是UTM投影的第31带。两个投影都是按照分带方法各自进行投影，故各带坐标成独立系统。以中央经线投影为纵轴，赤道投影为横轴，两轴交点即为各带的坐标原点，为了避免出现负值，高斯-克吕格投影与UTM北半球投影中规定将坐标纵轴西移500公里后当做起始轴。而UTM南半球投影除了将纵轴西移500公里外，还将横轴南移了10000公里。为了区别每一个坐标系统属于哪一带，通常再横轴做标签加上带号。

## Qt的button()和buttons()  

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

## google搜索引擎字符串  

> 20190316

1. google搜索引擎:  
`https://www.google.com/search?{google:RLZ}{google:acceptedSuggestion}{google:originalQueryForSuggestion}{google:searchFieldtrialParameter}{google:instantFieldTrialGroupParameter}sourceid=chrome&ie={inputEncoding}&q=%s`

## double类型的精度  

> 20190320

1. C#中的一个数据类型`decimal`  

2. 不要依赖调试器观察变量的值,要依赖`printf`,`qDebug()`,`Console`语句的输出.对于double类型的变量,对这个问题有者切身的体会.示例:`double a=0.3102`,通过调试器观察该值,会发现,`a=0.310199999999998`,但是输出到控制台中,还是为`0.3102`.具体原因不是很明白,可能是由于double类型使用二进制来存储.

3. 在**C#**中有`decimal`数据类型可以使用,但是在**C++**中没有,可以通过个折中的方法来实现.

   ``` c++
   double a = 7218612, b = 60000;
   double c = (double)(a) / b;//此处,c 实际为 120.3102.
   qDebug() << c;
   //但是通过调试器来查看值,c=120.310199999998.使用qDebug()输出到控制台中的数值为120.31
   double d = 120.3102;
   qDebug() << d; //此时d=0.3102,输出到控制台中的值也是120.3102
   //折中的解决办法.
   int e = a / b;//e = 120
   int f = a % b;//f = 186120
   double g = double(f) / (double)(b);//g = 0.3102
   QString h = QString::number(g);//h = "0.3102"
   h = QString("%1.%2").arg(e).arg(h.remove("0."));//h = "120.3102"
   ```

4. 换一种思路

   > 将double类型的数据转换成QString字符串类型,总是会损失精度,那么就不转换,如果需要传值,可以将两个除数和被除数一块传递过去,等需要使用的时候在计算double.

   ``` c++
   //如果需要计算7218612/60000的值,那么可以这样
   QString double_str = "7218312/60000";
   //当需要使用的时候,可以这样.
   QStringList list = double_str.splite("/");
   double double_double = list[0].toDouble() / list[1].toDouble();
   //此时double_double的值就是120.3102,虽然通过调试器观察到的值还是120.3101999998,但是实际是120.3102.
   ```

## QRegExp的exactMatch问题

> 20190321

1. 直接上示例代码

   ```Qt
   QString string = "123145178";
   QRegExp re("^1");
   bool ismatch = re.exactMatch(string);//此时,ismatch=false  
   QRegExp re2("^1.*");
   bool ismatch2 = re2.exactMatch(string);//此时,ismatche2=true
   ```

   > 通过上面的代码可以理解,`exactmatch`函数会判断string的整体与QRegExp是否会匹配,而不是string的一部分能与QRegExp匹配.

## c++中的const(未完待续)

> 20190322

1. const的含义

## Qt中QGraphicsItem的boundingRect的值

> 20190322

1. 自定义实现图形项(QGraphicsItem)时,有一个纯虚函数(boundingRect)

   > boundingRect()函数将图形项的外部边界定义为一个矩形,所有的绘图操作都必须限制在图形项的边界矩形中.而且,QGraphicsView要使用这个矩形来剔除那些不可见的图形项,还要使用他来确定绘制交叉项目时哪些区域需要进行重新构建.另外,QGraphicsItem的碰撞检测机制也需要使用到这个边界矩形.如果图形绘制了一个轮廓,那么在边界矩形中包含包含画笔宽度的影响是很重要的.  

   ```Qt
   QRectF boundingRect()
   {
       qreal penWidth = 1;
       return QRectF(0 - penWidth /2 , 0 - penWidth / 2,
                     20 + penWidth, 20 + penWidth);
   }
   ```

   左上角减去半个画笔的宽度很容易理解,但是右下角为何要加上一个画笔宽度,而非半个画笔宽度.

## Qt的"<<"的使用技巧

> 20190322

1. 向QPolygon之类的数据类型中赋值.

   ```Qt
   QVector<int> vector_int1;
   vector_int1 << 1 << 2;

   QVector<int> vector_int2 = QVector<int>() << 1 << 2;

   //上面vector_int1和vector_int2是等价的,表面上看第二种写法可能多输入一次"QVector<int>".
   //但是,如果输入的实参是QVector<int>的话,使用第二种写法可以省略声明变量.
   void SumAllInt(QVector<int>);
   那么调用的时候可以这样写:(主要QVector<int>后面的括号)
   SumAllInt(QVector<int>() << 1 << 2 << 3);

   ```

## Qt的警告信息`QPainter::end: Painter ended with 2 saved states`

> 20190326

1. 出现这个问题的原因: 在使用painter的过程中,有一处没有配对出现save()和restore()方法造成的.

## GDAL的警告信息`ERROR 1: Invalid index : -1`

> 20190326

1. 出现这个问题的原因之一是使用`OGRFeature::GetFieldAsString(const char*)`等等的`GetFieldAs...(const char*)`的函数,凡是海图文件中没有`const char*` 所表示的属性名称.

2. 解决办法:

   ```Qt
   OGRFeature* feature;
   //feature已经定义
   QString value = feature->GetFieldAsString("ORIENT");
   //如果该feature中没有ORIENT这个属性,就会出现警告"ERROR 1: Invalid index : -1"

   //应该先判断feature中是否有这个属性,在进行读取.
   int index = feature->GetFiedlIndex("ORIENT");
   if(index != -1){
       value = feature->GetFieldAsString("ORIENT");
       //也可以
       value = featuer->GetFieldAsString(index);
   }

   //如果简化代码的话,可以这样
   if(feature->GetFieldIndex("ORIENT") != -1){
       value = feature->GetFieldAsString("ORIENT");
   }
   //这种方法定义了一个变量.
   ```

## Qt中的ui设计师界面中转到槽

> 20190327

1. 在Qt的ui设计师界面中,添加一个pushButton,右键转到槽,会自动添加以下代码

   ```Qt
   //在.h文件中
   private slots:
       void on_pushButton_clicked();
   //在.cpp文件中
   void <classname>::on_pushButton_clicked()
   {
       //
   }
   ```

   监测整个写入代码的流程,没有发现`connect`的函数将点击事件与槽函数连接起来.应该是Qt会根据命名规则自动连接空间的点击事件和槽函数.自定义槽函数的时候,不要与Qt的命名规则冲突.

2. 以下来自Qt的官方文档

   `void QMetaObject::connectSlotsByName(QObject *object)`  
   Searches recursively for all child objects of the given *object*, and connects matching Signals from them to slots of *object* that follow the following form.  
   `void on<object name>_<signal name>(<signal parameters>);`  
   Let's assume our object has a child object of type *QPushButton* with object name *button1*.The slot to catch the button's *clicked()* Signal would be:  
   `void on_button1_clicked();`  
   If object itself has a properly set object name,its own signals are also connected to its respective slots.
   **See also** QObject::setObjectName();

## Qt的QComboBox控件的显示问题

> 20190329

1. 使用Qt Creator中的ui设计器,在widget中添加一个`QComboBox`控件,添加项目(Item),第一个为"FALSE",第二个为"TRUE".

   ```Qt
   //QComboBox的名称为combobox
   combobox->setCurrentIndex(1);//显示TRUE
   combobox->setCurrentText("FALSE");//显示FALSE
   //第一个项目为默认值,如果设置当前的文本没有在QCombobox中的列表中找到,会显示第一个位置的文本,即index(0)的内容.
   combobox->setCurrentText("F");//FALSE.
   ```

## Qt和VS同时编程

> 20190415

1. 在使用Qt编程的情况下,并且使用vs进行调试(因为qt的调试功能不如vs的强大),不要同时使用vs和qt同时打开一个工程,否则,会出现一个问题

   在vs中调试的过程中,发现错误,并修改保存,退出;此时,如果需要关闭Qt,当Qt处于焦点状态的时候,会出现弹窗,询问:  
   **The file mapcanvas.cpp has been changed on disk. Doyou want to reload it?**  
   **Yes ; Yes to All ; No ; No to All ; No to All &Diff; 显示细节…… ; Close ;**(此处为选项)  
   如果没有reload,并且还重新保存了,那么,之前修改的所有内容会被修改前的内容覆盖,而且再也找不回来了(如果修改后的版本没有上传到版本管理工具的话).

## C++声明

 > 20190422

 1. 使用C++声明一个函数的时候, 形参名称可以省略,例如下面:  
    `void GetValue(int, int)`

    但是, 没有名称, 谁知道这两个int表示什么含义. 对该函数注释的话, 没有形参名称, 那么只能"第一个int表示..., 第二个int表示...". 相反, 加上形参名称就可以一目了然: "name1表示..., name2表示..."  
    `void GetValue(int name1, int name2`

## 使用QColor的过程遇到的QMap的一个易错点

> 20190422

1. 下面是代码

   ```Qt
    QMap<QString,QColor> mapcolor;
    mapcolor["red"] = QColor(255,0,0);
    mapcolor["green"] = QColor(0,255,255);
    QMap<QString,QColor>::iterator iter = mapcolor.find("red");
    QPainter p(this);
    QPen pen;

    //片段一
    pen.setColor(iter->value());//只有这一行不同
    p.setPen(pen);
    p.drawText(20,20,QString("red:%1, green:%2, blue:%3.")
               .arg(pen.color().red())  //值为0;
               .arg(pen.color().green())  //值为0;
               .arg(pen.color().blue()));  //值为255;

    //片段二
    pen.setColor(iter.value());//只有这一行不同.
    p.setPen(pen);
    p.drawText(20,50,QString("red:%1, green:%2, blue:%3.")
                .arg(pen.color().red())  //值为255;
                .arg(pen.color().green())  //值为0;
               .arg(pen.color().blue()));  //值为0;
   ```

2. 上面代码中有两个片段, 其中片段一和片段二只有一个符号不同, 就是**iter**后面的 **->** 和 **.**; 但是结果截然不同.使用 **->** 所匹配的结果(颜色)显然是不正确的, 但是使用Qt Creator没有提示语法错误; 使用 **.** 所匹配的结果才是正确的. 但是当QMap中没有QColor时,使用 **->** 就是提示语法错误, 暂时不知是QMap的错误还是有隐藏的用法.

## Qt断言

> 20190423

1. Qt断言中可以使用两个宏 **Q_ASSERT** 和 **Q_ASSERT_X**.  

   ```Qt
   bool ass = false;
   //Q_ASSERT()出现错误的弹窗.显示以下信息.
     //Debug Error!
     //Program:一般为.dll文件路径.
     //Modele:Qt版本号.
     //File:该断言所在文件的绝对路径.
     //Line:该断言所在文件的行数.
     //ASSERT:"ass" in file d:\qt\main.cpp,line 53
     //(Press Retry to debug the application)
   Q_ASSERT(ass);  //如果点击忽略,可继续执行断言后的代码.

   //Q_ASSERT_X()出现错误弹窗.显示的信息几乎相同,下面仅列出不同的信息.
   //ASSERT failure in ass: "ass = 0", file d:\qt\main.cpp,line 53
   //其余显示的信息完全相同.
   Q_ASSERT_X(ass, "ass", "ass = 0");
   ```

2. 断言仅在Debug模式下启用,如果在Release模式下,不会启用.

3. 断言和异常处理不同,不能混为一谈.

assert用在那些你知道绝对不会发生的事情上,但是因为人总是会犯错误,保不准你写出来的东西跟你想的不一样.所以assert用来捕捉的是程序员自己的错误.  
同理,exception捕捉的是用户或者环境的错误.

## C++接口(抽象类)

> 20190425

   C++接口时使用**抽象类**来实现的. 抽象类中至少有一个函数被声明为**纯虚函数**.

1. 虚函数和纯虚函数

   关键字: **virtual** , 在类中正常声明的成员函数的前面加上virtual, 则为虚函数; 如果, 一个虚函数的声明 **= 0** ,则为纯虚函数, `virtual 返回值数据类型 函数名() = 0;`; 如果, 类中至少有一个函数被声明为纯虚函数, 则该类就是**抽象类**.  

2. 继承

   直接上代码.

   ```C++
    //基类
    class Shape
    {
     public:
        Shape() {};
        virtual int getarea() = 0;
        //virtual int getarea() { return 0; }
    };

    //派生类
    class Rectangle : public Shape
    {
     public:
        //纯虚函数的实现.
        int getarea() { return 4; }
        int special() { return -1;}
    };
    class Triangle : public Shape
    {
     public:
        //纯虚函数的实现.
        int getarea() { return 3; }
    };

    int main()
    {
        //着重注意这一句代码.
        Shape* shape[2] = { new Triangle(), new Rectangle() };

        std::cout << "Triangle:" << shape[0]->getarea() << std::endl;
        std::cout << "Rectangle:" << shape[1]->getarea() << std::endl;
    }
   ```

   上面main函数中的第一句代码`Shape* shape[2] = { new Triangle(), new Rectangle() };`, 定义了一个数组,这里包含了几个点需要说一下:

   - 纯虚函数不能被实例化.
   - 派生类的实例化: 派生类的构造函数的执行前,会首先执行基类的构造函数,可以使用**new**关键字来使用派生类对基类的实例化. 但是不能使用基类对派生类的实例化. 要就是不能`Triangle* triangle = new Shape();`, 这句代码是错误的.
   - 关于Shape中的纯虚函数: `getarea()`是纯虚函数, 那么, 在上面的代码中的**main**函数中, **Shape[0]**是Triangle类的实例化, **Shape**是Rectangle类的实例化. 如果, Shape中纯虚函数改为虚函数, 虽然输出的结果相同, 都是执行的Rectangle::getarea(), 但是Shape类可以实例化. 此时, Rectangle::getarea()称为Shape::getarea()函数的重新定义, 而非之前的虚函数的实现.
   - 关于`Shape[0]->special();`, Shape[0]中不存在special()函数.

## 初始化(未完待续)

>20190429

1. 变量初始化

   变量初始化有两种形式, 一种是赋值运算符初始化, 一种是括号初始化.  

   ```c++
   int a;
   a = 10;  //赋值运算符初始化.
   int b(10);  //括号初始化.
   //括号初始化,只能在定义变量时的初始化.
   int c;
   c(10);  //这种会出现编译错误.编译器会认为,c是一个函数, '10'是实际参数.
   ```

2. 类中的初始化

## Qt枚举类型与字符串类型相互转化(Qt5以后的版本)

> 20190506

1. 在Qt中将枚举类型注册(QT_ENUM或QT_Q_FLAG)后, 就可以利用Qt的元对象进行枚举类型与字符串类型 转化了.

   ```Qt
   #include <QtCore/QMetaEnum>

   int main(){
       QMetaEnum meta_enum = QMetaEnum::fromType<Qt::Alignment>();
       //字符串转为枚举值
       Qt::Alignment alignment = (Qt::Alignment)meta_enum.keyToValue("Qt::AlignLeft");
       alignment = (Qt::Alignment)meta_enum.keysToValue("Qt::AlignLeft | Qt::AlignVCenter");
       //枚举值转为字符串
       const char* s = meta_enum.valueToKey(Qt::AlignCenter);
       return 0;
   }
   ```

2. 在qss中我们可以这样使用枚举类型

   ```Qt
   QTabBar#CustomTabBar {
       //Qt::AlignmentFlay定义
       qproperty-text_align: "AlignLeft | AlignVCenter";
   }
   ```

3. Qt中判断信号是否与槽连接

   ```Qt
   QPushButton btn;
   int receivers = btn.receivers(SIGNAL(clicked(bool)));
   ```

## 编程学习之路

>20190520

1. 计算机组成原理
2. DOS命令
3. 汇编语言
4. C语言(不包括C++), 代码书写规范
5. 数据结构, 编译原理, 操作系统
6. 计算机网络, 数据库原理, 正则表达式
7. 其他语言(包括C++)
8. 架构......

## qSort(Iterator begin, Iterator end, bool lessThan)

>20190521

1. qt文档对此函数的描述

   This is and overloaded function.  
   Use std::sort instead.  
   Uses the lessThan function instead of operator<() to compare the items.  
   For example, here's how to sort the strings in a QStringList in case-insensitive alphabetical order:
  
   ```Qt
   bool caseInsensitiveLessThan(const QString& s1, const QString& s2)
   {
       return s1.toLower() < s2.toLower();
   }

   int doSomething()
   {
       QStringList list;
       list << "Alpah" << "beTA" << "gamma" << "DELTA";
       qSort(list.begin(), list.end(), caseinsensitiveLessThan);
       // list: [ "Alpha", "beTA", "DELTA", "gamma" ]
   }

   // 如果doSomething是类中的函数, 那么caseInsensitiveLessThan必须在类外声明, 否则会出现无法识别标识符caseInsensitiveLessThan.
   ```

## c++计算每个函数的执行时间(clock_t)

>20190527

```c++
clock_t startTime,endTime;
    startTime = clock();
    for (int i = 0; i < 1000000; i++)
    {
        i++;
    }
    endTime = clock();
    //函数执行所用的时间:
    double thetime = endTime - startTime;
```

## 关于代码格式化(未完)

>20190606

1. qt中代码格式化

---

## google搜索技巧

>20190609

### 关键字

1. 双引号

   把搜索词放在双引号中，代表完全匹配搜索，也就是说搜索结果返回的页面包含双引号中出现的所有的词，连顺序也必须完全匹配。bd和Google 都支持这个指令。例如搜索： “seo方法图片”

2. 减号

   减号代表搜索不包含减号后面的词的页面。使用这个指令时减号前面必须是空格，减号后面没有空格，紧跟着需要排除的词。Google 和bd都支持这个指令。例如：搜索 -引擎返回的则是包含“搜索”这个词，却不包含“引擎”这个词的结果

3. 星号

   星号*是常用的通配符，也可以用在搜索中。百度不支持*号搜索指令。比如在Google 中搜索：搜索*擎其中的*号代表任何文字。返回的结果就不仅包含“搜索引擎”，还包含了“搜索收擎”，“搜索巨擎”等内容。

4. inurl

    inurl指令用于搜索查询词出现在url 中的页面。bd和Google 都支持inurl 指令。inurl 指令支持中文和英文。比如搜索：inurl:搜索引擎优化返回的结果都是网址url 中包含“搜索引擎优化”的页面。由于关键词出现在url 中对排名有一定影响，使用inurl:搜索可以更准确地找到竞争对手。

5. inanchor

   inanchor指令返回的结果是导入链接锚文字中包含搜索词的页面。百度不支持inanchor。比如在Google 搜索 ：inanchor:点击这里返回的结果页面本身并不一定包含“点击这里”这四个字，而是指向这些页面的链接锚文字中出现了“点击这里”这四个字。可以用来找到某个关键词的竞争对收，而且这些竞争对手往往是做过SEO 的。研究竞争对手页面有哪些外部链接，就可以找到很多链接资源。

6. intitle

   intitle指令返回的是页面title 中包含关键词的页面。Google 和bd都支持intitle 指令。使用intitle 指令找到的文件是更准确的竞争页面。如果关键词只出现在页面可见文字中，而没有出现在title 中，大部分情况是并没有针对关键词进行优化，所以也不是有力的竞争对手。

7. allintitle

   allintitle搜索返回的是页面标题中包含多组关键词的文件。例如 ：allintitle:SEO 搜索引擎优化就相当于：intitle:SEO intitle:搜索引擎优化返回的是标题中中既包含“SEO”，也包含“搜索引擎优化”的页面

8. allinurl与allintitle

   类似。allinurl:SEO 搜索引擎优化就相当于 ：inurl:SEO inurl:搜索引擎优化

9. filetype

   用于搜索特定文件格式。Google 和bd都支持filetype 指令。比如搜索filetype:pdf SEO返回的就是包含SEO 这个关键词的所有pdf 文件。

10. site

    site是SEO 最熟悉的高级搜索指令，用来搜索某个域名下的所有文件。

11. related

    related指令只适用于Google，返回的结果是与某个网站有关联的页面。比如搜索`related:http://cnseotool.com`我们就可以得到Google 所认为的与点石网站有关联的其他页面。 这种关联到底指的是什么，Google 并没有明确说明，一般认为指的是有共同外部链接的网站。

12. linkdomain

linkdomain指令只适用于雅虎，返回的是某个域名的反向链接。雅虎的反向链接数据还比较准确，是SEO 人员研究竞争对手外部链接情况的重要工具之一。比如搜索`linkdomain:http://cnseotool.com -site:http://cnseotool.com`得到的就是点石网站的外部链接，因为`-site:http://cnseotool.com` 已经排除了点石本身的页面，也就是内部链接，剩下的就都是外部链接了。

## 隐藏逻辑和明确代码

>20190610

1. 这里有一个疑问, 直接上代码:

   ```Qt
   bool ismatch = false;
   int lineindex = -1;
   QList<??> matchinglines;  // 这是一个QList容器, 里面的内容不具体考虑.
   for (int i = 1; i < matchinglines.length(); ++i) {
     ismatch = IsMatching(matchinglines.at(i));  // return true or flase;
     if(ismatch) {
       // 匹配matchinglines[i].
       lineindex = i;
       break;
     }
   }
   if (!ismatch) {
     // 匹配matchinglines[0].
     lineindex = 0;
   }
   return matchinglines.at(lineindex);
   ```

2. 大体思路是:

   遍历QList容器, 如果IsMatching == true, 那么, 获取QList中的特定元素; 如果IsMatching == false, 那么, 获取QList中的第一个元素.

3. 那么, 使用隐藏的逻辑, 还是, 使用明确的代码?

   方案一: 明确代码, `lineindex`的默认值为`-1`, 最后还有一步`lineindex = 0`;

   方案二: 隐藏逻辑, 令`lineindex = 0`, `for循环遍历`, 没有最后一步对`lineindex`重新赋值为`0`.

   哪一种方案好呢?

## qt调试过程进入源码

>20190611

1. 首先下载源码

   下载的源码好像有两种, 一个是700多M, 一个是2个多G, 都能单步进入源码调试, 但是效果大不相同. 应该是下载2个多G的那个.

2. 下载PDB文件

   由于5.11.1版本Qt的dll和pdb文件是分开的, 需要在安装完成Qt后单独下载. 找到下载Qt安装包的位置,pdb文件的名称为 `qt-opensource-windows-x86-pdb-files-desktop-5.11.1.7z`, 下载并解压该文件. 解压出3个文件夹`msvc2015, msvc2015_64, msvc2017_64`, 找到Qt安装路径(默认为C:\Qt\Qt5.11.1\Qt5.11.1), 根据需要选择解压出来的三个文件夹, 复制到Qt安装路径下.

   **注意**, 虽然解压出来的3个文件夹内还有递进的文件夹, 只需选中`msvc2017_64`层级的文件夹, 复制Qt安装路径夹下即可, 会自动复制到Qt安装路径下原有的`msvc2017_64`文件夹中对应的位置(包括整个文件结构).

3. Qt Creator调试Qt源码

   依次打开菜单栏`Tools -> Options -> debugger -> General`, 可以发现大片空白的地方, `Source Paths Mapping`, 点击添加`Add Qt sources`, 选择安装qt的位置, 默认是在`c:\Qt\Qt5.11.1\Src`, 确定后, 自动出现`c:/work/build/qt5_workdir/w/s`等等四行路径. 点击`Ok`完成.

4. VS调试Qt源码

   使用VS打开pro工程(Qt工程), 在解决方案资源管理器中右键解决方案 -> 属性 -> 通用属性 -> 调试源文件, 在`包含源代码的目录`中添加Qt源代码所在的路径(`c:\Qt\Qt5.11.1\Src`), 点击确定即可完成.  

   **注意**, 在解决方案资源管理器中, 有容易弄错的地方, 一个是解决方案, 一个是项目. 在项目中右键->属性,没有调试源代码的选项. 解决方案有"解决方案"四个字.

5. 使用F11单步调试即可

## qt中将QString转化成const char\*的注意事项

>20190618

1. 将QString变量转化成const char\*变量

   ```Qt
   QString str = "ABCDE";
   // 将QString一步到位地转化成const char*会出错. 转换出来的内容是乱码.
   const char* cch = str.toLatin1().data();
   // 正确的转化方式是先将QString转化成QByteArray,然后转化成const char*.
   QByteArray ba = str.toLatin1();
   const char* ch = ba.data();
   ```

2. 在形参列表中

   当函数中的形参为const char\*时, 实参可以将QString直接一步到位地转化成const char\*.

   ```Qt
   // 在.h文件中
   void ConvertQstring(const char*);
   // 在.cpp文件中
   // 调用ConvertQstring函数
   QString str = "ABCDEF";
   ConvertQstring(str.toLatin1().data());
   ```

## qt中QString::arg的注意事项

> 20190619

1. ~~QString::arg(const QString& a1, const QString& a2)~~

   疑似Qt的bug, 之后再运行, 好像就没有出现下面`%2`的效果了.

   ```Qt
   QString a1 = "a";
   QString a2 = "b";
   QString b = '\0';
   QString result = '\0';
   result = QString("%1%2").arg(a1, a2);        // result = "ab".
   result = QString("%1%2").arg(a1).arg(a2);    // result = "ab".
   result = QString("%1%2").arg(a1, b);         // result = "a%2".
   result = QString("%1%2").arg(a1).arg(b);     // result = "a".
   // 上面代码可以看出, 如果不确定添加到QString是否为空, 那么最好是使用多个arg来逐一添加.
   ```

2. 官方Qt文档中提到的一个注意事项

   ```Qt
   QString str;
   str = "%1%3%2";
   str.arg("hello").arg(20).arg(50);  // returns "hello500"

   str = "%1%2%3";
   str.arg("hello").arg(50).arg(20);  // returns "hello5020"
   // Let's look at the substitutions:
   // First, Hello replaces %1 so the string becomes "Hello%3%2".
   // Then, 20 replaces %2 so the string becomes "Hello%320".
   // Since the maximum numbered place marker value is 99, 50 replaces %32.
   // Thus the string finally becomes "Hello500".
   ```

## VS使用Qt编程时'未找到xx函数的定义'的一个奇怪的问题

1. 删除中文注释, 改用英文注释.

   当出现这个问题时, 一定是在文件中有中文的注释, 最简单的方法是**删除中文注释, 改用英文注释".

2. 改变文件编码格式

   如果必须使用中文注释, 那么最好使用utf8的编码格式. Qt默认的编码格式就是utf8, 而中文语言系统下的window中的VS的默认编码格式为GBK. 当使用Qt或VS新建文件时, 文件的编码格式都是默认编码格式. 所以, 最好是在Qt(Qt Creator)中新建文件.

3. 将使用的宏替换

   有时在VS中会遇到一个奇怪的问题, 那就是所有的文件编码格式都改为了utf8, 但是在VS的头文件中还是会提示**未找到xx函数的定义**, 有些函数会提示, 有些函数不会提示; 在Qt中则不会出现这个提示. 我遇到了这个问题, 查找原因, 是因为我使用了**foreach(variable, container)**这个宏. 查看Qt帮助文件对foreach宏的说明, `Since Qt 5.7, the use of this macro is discouraged. It will be removed in a future version of Qt. Please use C++11 range-for, possibly with qAsConst(), as needed.`. 这里提到, foreach不推荐使用. 所以大胆的替换. 将foreach替换后, VS就不再会提示**未找到xx函数的定义**这个问题了, 显然, 问题已经解决. 可能除了foreach之外, 还有其他宏会导致这个问题.
