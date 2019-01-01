# HTML5小游戏设计与制作

## 游戏策划

楔子（setting）：回家路上，一只粗心的小鸟身上的金币散落一地，此时一只路过的小精灵热心地帮助小鸟把散落的金币收集起来，归还给小鸟。

人设与道具（Gameplay）：
1. player：小精灵。能通过键盘的方向键控制移动，帮助小鸟收集散落的金币。
2. bird：小鸟。等待小精灵收集金币，然后回家。
3. coin: 金币。小鸟散落的金币。

## 游戏设计CRC卡片

1.  Object: 小精灵
    Attribute: 图片，位置
    Collaborator: 金币 
    Events & Actions: 碰撞&销毁金币,得分加一

2.  Object: 小精灵
    Attribute: 图片，位置
    Collaborator: 小鸟 
    Events & Actions: 碰撞&游戏结束

3.  Object: 小鸟
    Attribute: 图片，位置
    Collaborator: Null
    Events & Actions: Null

4.  Object: 金币
    Attribute: 图片，位置
    Collaborator: Null
    Events & Actions :Null

## 游戏成果展示

![HTML5小游戏](https://img-blog.csdnimg.cn/20181231234807409.gif)



_Thanks for your reading!_

