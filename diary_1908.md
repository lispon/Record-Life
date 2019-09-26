# 201908

## QPen

> 20190815

### QPen-问题描述

 在Qt视图框架中, 使用QPen绘制的图形在缩放过程中, 图形的线宽也会随之变化, 需要设置固定线宽, 但是图形也需要随QGraphicsView的缩放而缩放.

### QPen-解决办法

1. 图形需要随QGraphicsView缩放, 所以不能使用`QGraphicsItem::SetFlag(QGraphicsItem::ItemIgnoresTransformations)`

2. 图形的线宽如果全部为1个像素, 那么可以设置线宽为0, 即`QPen::setWidth(0)`, 当线宽为0时, 在任意缩放情况下, 都会是一个像素.

3. 图形的线宽为任意像素, 那么需要设置cosmetric, 即`QPen::setCosmetric(true)`. 在任意缩放情况下, 线宽不会变化.

## git commit

> 20190815

### Git-问题描述

1. 使用git提交信息时, 最好不要直接使用git commit -m \<message\>, 而是单独写提交信息.

2. 使用svn提交信息时, 可以建立log模板.

### Git-解决方法(暂时未测试)

1. git

   1.1 在`.gitconfig`中添加`[commit] \n template = ~/.gitmessage`

   1.2 新建`~./gitmessage`文件, 内容可以如下:

   ```C++
   # head: <type>(<scope>): <subject>
   # - type: feat, fix, docs, style, refactor, test, chore
   # - scope: can be empty (eg. if the change is a global or difficult to assign to a single component)
   # - subject: start with verb (such as 'change'), 50-character line
   #
   # body: 72-character wrapped. This should answer:
   # * Why was this change necessary?
   # * How does it address the problem?
   # * Are there any side effects?
   #
   # footer: 
   # - Include a link to the ticket, if any.
   # - BREAKING CHANGE
   #
   ```

2. svn

   2.1 在已经检出的文件夹中,右键->属性, 新建->其他.
   2.2 在属性值中选择`tsvn:logtemplate`, 在下面的值中填入提交信息的模板.

## Qt中鼠标事件的获取

> 20190820

### Qt鼠标事件-问题描述

1. 新建QMainWindow工程; 新建MapCanvas类, 继承自QWidget; 将QMainWindow中的centralWidget的类由QWidget改为MapCanvas.

2. 此时, 在centralWidget的区域中无法进入鼠标移动事件, 只有使用按下鼠标拖动时, 才会进入鼠标移动事件.

### Qt鼠标事件-解决办法(暂未解决)

1. Qt帮助文档中关于QMouseEvent的Detailed Description里说的很清楚, `Mouse move events will occur only when a mouse button is pressed down, unless mouse tracking has been enabled with QWidget::setMouseTracking().`

2. 如果是在 `QMainWindow` 中, 需要 `centralWidget->setMouseTracking(true)`, 来开启监听鼠标移动事件.

## Qt中QMainWindow中的工具栏功能的实现问题

> 20190821

### 工具栏按钮状态的监听

1. 工具栏中有大量的按钮, 首先需要设计一个**枚举**来存取所有的按钮, 来判断是否那个按钮处于按下的状态. 设计枚举时, 可以借鉴Qt中枚举值的设计`0x0001, 0x0010, 0x0100, 0x1000`, 可以判断多个按钮的按下情况.

2. 然后, 再通过鼠标或键盘来触发, 进而执行功能代码.

## Qt中QPicture

> 20190826

1. QPicture::save及QPicture::load函数

   load和save函数中的QString是文件的路径, 而非仅仅是文件名称. 可以使用`c:/user/desktop`或`c:\\user\\desktop`格式的路径, 如果没有该文件夹, 不会自动根据路径新建文件夹.

2. QPicture::save和load函数不需要一一对应

   只要存在QPicture生成的二进制, 就可以使用QPicture::load函数; 同理, 可以只使用save函数.
