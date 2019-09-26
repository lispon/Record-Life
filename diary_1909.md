# 1909

## Qt 中以 2D 图形的每个点旋转特定的角度

> 20190902

1. 在 Qt 绘图过程中, 需要转换某个图形, 旋转中心点为图中的某个特定点.

   ```Qt
   // 无法运行, 需要根据实际情况稍加修改.
   QPainter painter(QPaintDevice *device);  ///< 如果一般在 paintEvent 中, 直接使用 QPainter painter(this);
   QPointF pivot(x, y);                     ///< 枢轴点坐标, 该坐标是在未变换前的坐标系下的坐标值.
   painter.translate(pivot);                ///< 注意, translate 的参数为坐标系的偏移量, 而非讲坐标系原点便宜到特定坐标.
   painter.rotate(qreal angle);             ///< 坐标系旋转, 如果 angle 为负, 那么逆时针旋转.
   painter.translate(-pivot);               ///< 在旋转后的坐标系中, translate 相反的偏移量.
   ```

2. 对整个操作过程进行解释

   首先, 使用translate将当前坐标系平移到需要的枢轴点位置, 然后, 旋转相应的角度. 此时, QPainter绘制图形时按照新的坐标系来进行绘制(即图形已经按照要求旋转). 那么, 当前图形中哪个点需要显示在旋转中心的位置呢? (在新的坐标系下, 旋转中心的位置就是原点), 如果图形中的点A(x,y)需要显示在旋转中心, 那么, translate(-x,-y)即可.

   **Note:** 这里还有一个问题, 这种方法, 只能使特定图形点显示在枢轴点的位置上, 如何将图形点显示在非枢轴点的位置上?

   ```Qt
   // 未解决.
   ```

## Qt中文路径

> 20190909

1. 如果路径中含有中文, 那么直接将路径转化成QString会出现乱码, 使用QString::fromLocal8Bit也会出现乱码. 但是, 将路径转化成QByteArray就不会出现乱码.
   这是一个奇怪的问题, 留待后续解决.
