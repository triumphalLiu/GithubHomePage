# 画板小工具
---
## 需求分析

- 绘制图形
- 编辑图形
- 辅助调整
- 流程图连线

---
### 需求——绘制图形
  
画直线、画矩形、画圆形、画三角形、以及其他形状  
  
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/1.png)<!-- .element height="50%" width="100%" -->
---
### 需求——编辑图形  
  
选中图形时，显示边框线及拉伸线  
  
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/2.png)<!-- .element height="50%" width="100%" -->
---
### 需求——编辑图形  
  
根据8个伸缩点，对图形进行自由拉伸  
  
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/3.png)<!-- .element height="50%" width="100%" -->
---
### 需求——辅助调整
  
移动图形的时候，需要显示辅助对齐线   
  
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/4.png)<!-- .element height="50%" width="100%" -->
---
### 需求——流程图连线
  
对每个图形之间用箭头进行连接   
   
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/5.png)<!-- .element height="50%" width="100%" -->
---
### 需求——流程图连线
  
此连线应当跟随父图形移动  
  
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/6.png)<!-- .element height="30%" width="100%" -->
---
  
## 遇到的问题  
  
- 图形移动时的闪烁
- 面向过程编程，代码臃肿
- 多显示器鼠标位置问题
- 判断点是否在图形上
  
---
  
### 问题——闪烁
  
原因A：刷新频次不够  
  
解决方案：  
- 当涉及到移动的附属操作时（如对齐线的显示），都会把正在移动的图形再绘制一次

---
  
### 问题——闪烁  
  
原因B：重绘时频繁调用了```Graphics.Clear(Color.White)```方法  
  
解决方案：  
- 除放缩等涉及到整个屏幕的变换时清屏重绘外，其他时候在原来的基础上重绘
- 对于正在移动的图形，先在画布上用背景色移动前的位置绘制，以达到删除图形的效果

---
### 问题——鲁棒性和可读性
  
解决方案：重构代码，将所有图形作为`GraphObject`的子类，每个类单独实现自身独特的移动和描绘。  
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/7.jpg)<!-- .element height="50%" width="100%" -->

---

### 问题——多显示器  

使用较多的两种移动鼠标的方法：  
- ``` SetCursorPos(Point) ``` 会默认地将主频左上角当作坐标`(0,0)`。  
- ``` Cursor.Position = Point ```，效果同上。   
  
---

### 问题——多显示器  

解决方案：使用偏移量，通过API:``` mouse_event(int flags, int dx, int dy, uint data, int extraInfo) ```可以实现模拟鼠标的相对运动。  
  
---

### 问题——判断点是否在图形上  
  
解决方案：  
- 对于含直线的图形，判断点是否在直线上；  
- 对于圆，判断点到圆心的距离；
- 对于曲线，选多个取样点，把两个相邻的取样点当作直线看待。  
  
---

### 问题——判断点是否在图形上  
  
更优的解决方案：  
- 通过图形学的方法，即过该点任意画一条射线，求出此射线与边界的交点
- 如果为偶数，则在图形外，奇数在图形内（若交点为顶点，则计2个交点）。  
![](https://github.com/triumphalLiu/Slides/raw/master/slides/cvte/screenshot/8.png)<!-- .element height="50%" width="100%" -->


---

## 感想
  
- 高效（Git）
- 设计模式（Object Oriented, OO）
- 总结（解决方案的复用）
- 思考问题的方式（换一种思路）
- 解决问题的方式（抽象工厂）

--- 
  
## 谢谢！