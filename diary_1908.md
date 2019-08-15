# 201908

## QPen

> 20190815

### 问题描述

 在Qt视图框架中, 使用QPen绘制的图形在缩放过程中, 图形的线宽也会随之变化, 需要设置固定线宽, 但是图形也需要随QGraphicsView的缩放而缩放.

### 解决办法

1. 图形需要随QGraphicsView缩放, 所以不能使用`QGraphicsItem::SetFlag(QGraphicsItem::ItemIgnoresTransformations)`

2. 图形的线宽如果全部为1个像素, 那么可以设置线宽为0, 即`QPen::setWidth(0)`, 当线宽为0时, 在任意缩放情况下, 都会是一个像素.

3. 图形的线宽为任意像素, 那么需要设置cosmetric, 即`QPen::setCosmetric(true)`. 在任意缩放情况下, 线宽不会变化.
