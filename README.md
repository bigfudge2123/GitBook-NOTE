# Runge Kutta草稿

## 一、方程

方程就是包含 **未知量**的等式，求解方程就是要透过现象看本质。

大家已经熟悉的代数方程 $$x^2+5x-6=0$$、三角函数方程 $$2sin\theta +3=0$$ ，这些方程的未知量是一个量的某几个特定的值。

但是在实际的应用中，很多未知量是一个函数，比如$$F(x,\phi(x))=0$$，$$\phi(x^2+1)=2\phi(x)$$，和$$\phi(\phi(x))=x$$。这其中的$$\phi$$是未知函数，这些方程被称为函数方程或者泛函方程。

而在这之中，联系着**自变量、未知函数、未知函数的导数**的函数方程称为微分方程，如$$\dfrac{d^2\phi}{dt^2}+k^2\phi+\gamma\phi^3=A\cos\omega t$$

这是一个常微分方程，其中的未知函数$$\phi(t)$$是关于自变量t的一元函数。

严格来说，常微分方程的一般形式是$$F(t,\phi,\phi^\prime,\cdots,\phi^{(n)})=0$$

其中，$$F$$是一个$$n+2$$元的已知函数，而$$\phi^\prime,\cdots,\phi^{(n)}$$是未知函数$$\phi(t)$$的一阶至阶导数，这个$$n$$为方程的阶数，称该方程为$$\textbf n$$**阶常微分方程**。

如果函数$$\phi(t)$$在区间$$J$$上连续,有直到$$n$$的导数，而且对所有的$$t\in J$$，一般形式恒成立，则称$$\phi(t)$$为方程？？在区间$$J$$上的一个解。

## **二、Euler法及改进的Euler法**

### 1. Euler法

对区间（求解区域）进行离散，对连续的微分方程的导数进行离散

用$$y{(x_i)}$$表示函数$$y(x)$$在$$x_i$$点的精确值，$$y_i$$表示近似值。

将区间$$a,b$$$$N$$等分，小区间长度$$h=\frac{T}{N}$$称为步长，点列$$x_i=a+ih(i=0,1,\cdots,N)$$称为节点，其中$$x_0=a$$。

用$$y_1$$作为$$y(x_1)$$的近似值。同样,利用$$y_1$$可算出$$y_2$$作为$$y(x_2)$$的近似值。递推下去便可算出$$y(x)$$ 在所有节 点上函数的近似值，递推公式为$$y_{n+1}=y_n+hf(x_n,y_n),n=0,1,\cdots,N$$

### 2. 改进的Euler法

用梯形的面积近似矩形的面积

$$
y_{n+1}=y_n+\frac{h}{2}(f(x_n,y_n)+f(x_{n+1},y_{n+1})),n=0,1,\cdots,N-1
$$

未知数同时出现在等式的两边，需要解方程才能求出$$y_{n+1}$$，这类算法称为**隐式算法**，相对应的就是显式算法。

对隐式算法,每步计算需要解关于$y\_{n+1}$的方程,这样的方程往往是非线性的,需用迭代法求解,常将初值取为$y\_{n+1}^{\[0]}=y\_n$一 般只需迭代几步即可收敛。

选取更好初值的方法是先用显式公式计算一个初值,再用隐式公式迭代求解,这样的方法称为预估-校正法。

#### 实际问题的微分方程模型

在许多问题中往往不能直接找出变量之间的函数关系,但是有时却容易找出 变量的改变量之间的关系,从而建立描述问题的微分方程模型。

从分析实际问题到提出这样的数学形式的过程叫做**数学建模**。

比如说我刚接了一杯水，它有100°，我把它放在这个教室，教室的环境温度是24${^\circ}$。如果说水的温度低于50°才能喝，那我现在想知道我要什么时候才能喝这一口？影响这杯水温度的自变量有哪些？

本身的温度、教室的温度、杯子的材质、杯子的形状、跟空气唯一的接触面的面积。

把一个动态的问题转化为相对静止的问题进行研究，这就是微分方程的意义。

数值解

|    $x$   | $x\_0$ | $x\_1$ | $\cdots$ | $x\_k$ | $\cdots$ | $x\_n$ |
| :------: | :----: | :----: | :------: | :----: | :------: | :----: |
| $y=f(x)$ | $y\_0$ | $y\_1$ | $\cdots$ | $y\_k$ | $\cdots$ | $y\_n$ |
