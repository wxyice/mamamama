<link rel="stylesheet" href="markit.css"></link>

# 运动的描述
<h2>目录</h2>

- [运动的描述](#运动的描述)
  - [基础概念](#基础概念)
    - [质点](#质点)
    - [参考系](#参考系)
    - [时](#时)
    - [路](#路)
      - [路程](#路程)
      - [位移](#位移)
    - [坐标系](#坐标系)
  - [速度](#速度)
    - [平均速率](#平均速率)
    - [瞬时速率](#瞬时速率)
    - [平均速度](#平均速度)
    - [瞬时速度](#瞬时速度)
  - [加速度](#加速度)


这一部分是物理的基础，需要充分理解。
## 基础概念
### 质点

**质点是什么**

当我们分析一辆车的运动的时候，我们是如何看的。假设这辆车沿着一条非常非常直的跑道往前开去，我们看看这辆车。如果我们看的是这辆劳斯莱斯的头上的小金人，它几乎是和车一起笔直的向前开去。但是当我们单独看车轮的时候，我们发现，车轮在向前动的同时，还在不停的旋转。在看坐在里面的人，那不止向前运动，还可能到处扭动。我们发现，如果我们仔细的描述这辆车的运动，几乎是不可能的，它非常的复杂。但是其实，我们不需要这么多的信息，我们往往只关注它开完跑道需要多久，或者这辆车现在在哪。基于这种简化的思想，物理学家们提出了一个数学模型来进行描述。这就是质点。<br>
OK，那么我们来非常学术的说一下这个概念<br>
质点是指一个只有质量的点

**怎么样可以称作质点**

前面说，质点是只有质量的点，那么很明显，质点没有形状和大小，更加没有所谓的内部结构（废话）。当我们所研究的物体没有如上特征(这句话感觉很不严谨)，或者我们不关心如上特征，或者相对于整个问题，这些特征不是很明显的时候，我们就可以把研究对象简化的看作是一个质点。比如我只想知道这辆劳斯莱斯花里多长时间完成了整个赛道，这个时候我们就可以看作是质点，如果我们想知道它是怎么漂移过每个弯的，想知道轮子怎么转的，这时候就不行。吐槽：劳斯莱斯当方程式赛车开？？？<br>
所以，是否可以看作是质点，不是看物体本身，而是看研究问题的。

- 物体大就不能看作质点[x]
- 物体小就一定看作质点[x]

**理想化模型**

我们迅速的引进这个概念，理想化模型。什么是理想化模型。一个突出主要矛盾，忽略次要矛盾的合理的数学模型，就可以说是理想化模型（我说的，可能还是要看看书理解一下，而且，这很马克思）

### 参考系
第二个很重要的概念——参考系

参考系是指，在描述一个物体运动的时候选来作为标准假定不动的另一个物体。可以好好理解一下我们在车上看到外面的树向后的例子，或者明明是旁边的车发动，反而感觉是自己动了。（当然这是有条件的，比如没有发动机震动这种“场外提示”）

### 时
关于时，有两个概念需要理解一下，一个是时间间隔，一个是时刻。每当我们说时间的时候，是模棱两可的（文本上）但是意思上明确的。比如两点半是只一个时刻，两小时就是时间间隔。（这应该很好理解）<br>
当然也可以结合数轴来理解。假设时间是一条“画不出零点的”数轴，由过去指向现在，显然数轴上的点表示时刻，数轴上的线段表示时间。

### 路
这里完全是为了强迫症的工整。接下来我们需要引入两个概念，分别是位移和路程。

#### 路程
从简单的地方开始，我们先理解路程。什么是路程。其实它和我们平时理解的是基本不冲突的，我们用学术一点的话说：路程是物体运动轨迹的长度。显然，绕了400米操场跑了两圈，如果有人和说你只跑了0米，一定会打起来，正常人会说自己跑了800米。这很合理。

#### 位移
但是位移量就是这么不合理。他确实可能位移为0。这里我们引入了一个新的量，位移。简单介绍一下。
- 位移
  - 性别：矢量
  - 年龄：我怎么知道什么时候提出来的这个量orz
  - 工作：在物理学中描述物体的位置变化

位移是矢量（可能是高中接触的第一个矢量）。由起点指向终点，不管质点的轨迹如何，它只和前后位置有关。
同时可以推出，两点之间，最短的路程长度是位移的模。

### 坐标系
不多说，分为一，二，三维。

## 速度
为了描述运动的快慢，我们引入了一个叫速度的物理量。但是这里面有不少新概念，为了不混淆，我们一个个看。

### 平均速率
这个和我们平时的概念冲突比较少。假设$t$时间里面开过$S$的路程，对于平均速率$V$有
$$V=\frac{S}{t}$$

### 瞬时速率
当我们把路程$S$变的很小很小很小很小，小到我们认为它可以替代表示当前时刻的速率，这就是瞬时速率。
$$V'=\frac{dS}{dt}$$

注：这是微分表达式，可以直接理解成，我在S前加了一个d表示这是一个很微小的量。（正确的理解请参考高等数学）

### 平均速度
平均速度是一个矢量，它是时间和该段时间位移的比值。假设位移$\vec{x}$，使用时间为$t$
数学表达式
$$\vec{v}=\frac{\vec{x}}{t}$$

方向与位移同向。

### 瞬时速度
把位移和时间变成小微元，直到它可以用平均速度表示为当前时刻的速度。
$$\vec{v}=\frac{d\vec{x}}{dt}$$

一般我们之后说速度都是指瞬时速度。当然也要看具体情景，可能不一样。

## 加速度
最后我们再引入一个量，加速度。
加速度是用来描述速度变化快慢的物理量。同样，它也是个矢量。根据我们上面速度的类比，我们直到肯定会有平均加速度和瞬时加速度的概念。
假设在$t$时间内，物体的速度变化了$\Delta \vec{v}$，那么平均加速度写出数学表达式
$$\vec{a}=\frac{\Delta \vec{v}}{t}$$
取小微元时间，其瞬时加速度为
$$\vec{a}=\frac{d\vec{v}}{dt}$$
$d\vec{v}$表示一个微小的速度变化量。

**思考**：有没有瞬时加速率和平均加速率？






