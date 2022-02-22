# 流线组件实现

## 一、 流线动画实现

### 1. VTK可视化技术 <a href="#1.-vtk-ke-shi-hua-ji-shu" id="1.-vtk-ke-shi-hua-ji-shu"></a>

{% embed url="https://blog.csdn.net/weixin_40798471/article/details/102814634" %}
animation
{% endembed %}

## 2. 基本概念

Time anchor 根据integration time 设置基于播种点的偏移

Time delta控制dash的长度

一个 circle 等于$Dash skip \* dash$的长度

steps per circle 是给定速率运行的步数

```
// Some code
auto pMarker = vtkSmartPointer<vtkActor>::New();

```

### 3. 其他相关

vtkAnimationScene
