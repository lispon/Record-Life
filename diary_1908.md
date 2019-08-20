# 201908

## QPen

> 20190815

### 问题描述

 在Qt视图框架中, 使用QPen绘制的图形在缩放过程中, 图形的线宽也会随之变化, 需要设置固定线宽, 但是图形也需要随QGraphicsView的缩放而缩放.

### 解决办法

1. 图形需要随QGraphicsView缩放, 所以不能使用`QGraphicsItem::SetFlag(QGraphicsItem::ItemIgnoresTransformations)`

2. 图形的线宽如果全部为1个像素, 那么可以设置线宽为0, 即`QPen::setWidth(0)`, 当线宽为0时, 在任意缩放情况下, 都会是一个像素.

3. 图形的线宽为任意像素, 那么需要设置cosmetric, 即`QPen::setCosmetric(true)`. 在任意缩放情况下, 线宽不会变化.

## git commit

> 20190815

### 问题

1. 使用git提交信息时, 最好不要直接使用git commit -m \<message\>, 而是单独写提交信息.

2. 使用svn提交信息时, 可以建立log模板.

### 解决

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
