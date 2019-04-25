# 学习日记  

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