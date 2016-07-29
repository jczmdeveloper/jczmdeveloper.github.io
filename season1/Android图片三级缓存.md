#自定义控件
---
##自定义View的步骤：

- 1.自定义View的属性
- 	在View的构造方法中获得我们自定义View的步骤
- 2.重写onMeasure（）(可选择)
- 3.重写onDraw（）


##自定义ViewGroup的步骤：

- 1.自定义ViewGroup的属性
- 	在ViewGroup的构造方法中获得我们自定义View的步骤
- 2.重写onMeasure（）(可选择)
- 3.重写onLayout()(可选择）
- 4.重写onDraw（）[注意：因为ViewGroup默认是不调用onDraw（）的，所以必须 设置 setWillNotDraw(false)来调用onDraw（）]
