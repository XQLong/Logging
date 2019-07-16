# 移动机器人路径规划
## 1.路径规划及其基本流程


### 1.1 路径规划概念

- 应用场景

身边的路径规划应用实例：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563027343428-1563027343433.png)

</div>

例如无人机避障，各种地图导航和打车软件，物流调度系统，自动驾驶技术里面都应用到了路径规划。

路径规划是机器人研究和核心内容之一，在很多领域都有广泛的应用，如室内服务机器人的自主运动；无人机的避障突防飞行；物流管理中的车辆问题及类似的资源管理、资源配置问题。


- 机器人移动路径规划

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1562989495555-1562989495562.png)

</div>

机器人移动的环境包括自由空间和障碍物占据的空间。移动机器人的路径规划便是要找到将机器人从开始位置到目标位置的连续路径，其中开始位置和目标位置必须位于机器人运动环境中的自由空间中。在路径规划中，移动系统使用已知的环境地图，该地图存储在机器人的存储器中。


### 1.2 环境感知

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563028348096-1563028348100.png)

</div>

机器人用自身携带的传感器感知环境，对环境形成两类信息，一类是静态的长期信息反映环境中静态障碍物的分布，另一类是动态的短期信息反映环境中动态障碍物的分布。

### 1.3 环境建模

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563017918277-1563017918284.png)

</div>

环境建模是为机器人的运行建立一个数学模型，有了环境地图信息，机器人就能实现目标识别、自定位、路径规划等动作。现实条件下机器人所处的环境是三维的，但是在机器人进行信息获取后，通过对环境的识别将三维环境内化为一个二维的地图，通过对这个二维地图的操作来对一整块区域实现覆盖的目标。环境又分为已知环境和未知环境：

已知环境：即机器人对即将覆盖的区域环境信息已知，这种情况下机器人不需要实时监控，在规划前就建立好环境模型，机器人直接执行路径搜索算法。已知环境建模主要运用的方法有栅格表示法、可视图法、拓扑图法、模板模型法等。

未知环境：即机器人所处的环境中，没有任何先验信息，如果已知的环境一旦变化，机器人所获取的环境信息也要根据环境的变化而变化，这样机器人在遍历过程中要根据不同环境信息对搜索路径进行规划，以达到最终目标。未知环境下可以通过机器人的传感器边走边建模（对机器人的灵敏度和反应速度要求较高），或者通过SLAM转换为已知环境利用已知环境的方法进行建模。

- 栅格表示法

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563021118607-1563021118611.png)

</div>

栅格表示环境地图是一种度量式表示环境的方法，机器人根据地图边界的大小量化成大小与自身等大的多个矩形，然后在所建立的环境地图上通过搜索算法进行路径规划。利用栅格法构建地图模型的步骤：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563019744979-1563019744985.png)
 
</div>

- 可视图法

可视图法是在二维情况下，起点S、障碍物的顶点以及目标点G连接，并确保所有直线段不穿过障碍物，然后得到了一张图，称之为可视图。可视图法中常用的是切线图法和维诺图法：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563021339983-1563021339988.png)

</div>

如图所示，在切线图法中从起点到目标点的连线不穿过障碍物的情况下，形成可视线，所有可视线中一定有一条从起点到目标点最短的路径，通过搜索算法判断确定后由机器人执行。考虑到机器人与障碍物之间的安全距离以及障碍物不规则等方面的因素，可先将障碍物进行矩形膨胀处理，再选取可视点和作可视线。

对于多角形状或者凹凸起伏较大的障碍物来说，如果矩形化可能造成造成障碍物区域的冗余，利用圆滑的曲线将障碍物完全包裹在一个封闭的图形中，封闭曲线的形状按照障碍物大体形状进行扩展，然后将所有的封闭曲线图形链接起来，其中相邻障碍物之间如果有相邻的边可以共用一条曲线，这样整体便形成一个维诺图:

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563022741114-1563022741124.png)

</div>

### 1.4 算法搜寻

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/14/1563069336804-1563069336809.png)

</div>

如上图所示，根据特定任务要求，路径规划通常分为点到点方式和全覆盖方式。

- 点到点的路径规划是指已知两点的情况下，在两点间寻找最优（如代价最小、路径最短、时间最短）路径。点到点包括对全局进行规划和对局部进行规划。全局路径规划注重寻求最优解，局部路径规划注重
- 全覆盖路径规划是指规划一条可行而且尽可能短的路径，使机器人在避开障碍物的同时又能使运行轨迹充满一整块区域。即控制移动机器人能够遍历工作区域中所有可达点，同时能保证自动避开障碍物。

### 1.5 执行算法

<div align="center">	

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1563025407696-1563025407700.png)	

</div>

通过以上步骤环境感知、环境建模、算法搜寻三个过程后，最后移动机器人执行系统执行该条路线，使机器人无碰撞的到达目标点。

## 2.A*算法

A*算法是结合了Djikstra算法和最佳优先算法两种算法优势提出的一种启发式搜索算法。它利用启发信息来引导搜索方向，从而减小搜索范围，提高搜索效率。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/14/1563076698852-1563076699032.png)

</div>

- 迪杰斯特拉算法的优势在于总可以找到最优化路径；
- 最佳优先算法能够快速引导向目标节点搜索，大幅提高搜索效率，但往往不能获取合理路径。

A*算法作为经典算法，以其实用性至今被广泛应用，并且针对不同的应用场景具有很强的扩展性和适应性。

### 2.1 Dijkstra算法

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563154458999-1563154459005.png)

</div>


迪杰斯特拉算法采用动态规划的思想，考虑如上图所示的问题，显然若有一条S0到Sn之间的最短（权重最小）通路，则从S0到该通路上的任意节点都是最短的。那么反过来，声明一个==数组dis==来保存源点到各个顶点的最短距离和一个保存已经找到了最短路径的顶点的==集合：T==，然后进行遍历搜索至Sn则必然能找到这条最短路径。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/13/1562978599408-1562978599414.png)

</div>

是典型的广度优先算法，优点在于总可以找到最优化路径；但是搜索效率低，难以满足快速规划路径的需求。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/14/1563101115619-1563101115625.png)

</div>

初始时，原点s的路径权重被赋为0（dis[s]=0）,若对于顶点s存在能直接到达的边（s，m），则把dis[m]设为w（s，m），同时把所有其他（s不能直接到达的）顶点的路径长度设为无穷大。如上图所示为迪杰斯特拉算法算法流程图，下面以一个具体实例来说明。

迪杰斯特拉算法示例：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/14/1563102366485-1563102366493.png)

</div>

如图所示，共有V1到V6的六个节点，箭头的方向表示路径为通路，箭头上的值表示该条线路的权值，现要求出节点V1到其他所有节点的权值最小路径。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/14/1563105174719-1563105174724.png)

</div>

在上图中“#”表示权值为无穷大，蓝色加粗表示从当前出发搜索路径，标红节点表示已知最小权重路径。根据上述算法流程便得到从V1节点到所有节点的最小权重的路径。

根据上述算法，在栅格法建模的地图中，

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563152598269-1563152598454.png)

</div>

如图所示，假设栅格长度为1，则可以将从一个栅格走到与其相邻栅格的权重（路径长度）为1，斜着通过对角线的权重为1.414，当然也可以根据环境的具体情况决定权重大小，例如有障碍物的情况则无法通行权重为无穷大。

根据迪杰斯特拉算法可以看到，在进行目标点最短路径的搜索过程中会毫无方向的向四周搜索，直到发现目标后即终止搜索。

无障碍物时的迪杰斯特拉算法的搜寻过程：


<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/Diskstra0.gif)

</div>

有障碍物时的迪杰斯特拉算法搜寻过程：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/Dijkstra.gif)

</div>

这样虽然总能找到最短的路径，但是算法的空间和时间复杂度都较大。

### 2.2 最佳优先算法（Best-First-Search）

与DijKstra算法最大的差异是，该算法并非选择离起点最接近的，而是以与目标节点之间的距离(权重)作为评估，这种算法能够快速引导向目标节点搜索，大幅度提高算法搜索效率；但是往往不能获取合理的最短路径。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/BFS0.gif)

</div>

最佳优先算法是一种贪心算法，它的基本思想是：每一次都选择“它认为离目标近”的点，不考虑起始点到当前节点已经花费的代价。

这里“它认为离目标最近”的点是通过一个评估函数（又称启发函数）计算出来的，“它认为离目标最近”的点不一定是最短路径上的点。

距离和启发函数：

我们一般可以采用常用的距离计算公式作启发函数，

- 曼哈顿距离
- 欧几里得距离
- 切比雪夫距离

在二维平面内，对于两点A（x1,y1）和B(x2,y2)

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563160113005-1563160113009.png)

</div>

欧几里得距离即最常用的两点间的直线距离;曼哈顿距离 dis=|x1-x2|+|y1-y2|;切比雪夫距离dis=max{|x1-x2|,|y1-y2|}。例如我们使用曼哈顿距离作为启发函数：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563160499373-1563160499379.png)

</div>

其中D为从一个位置移动到邻近位置的最小代价。

```
Best-First-Search( Maze m )
   Insert( m.StartNode )
   Until PriorityQueue is empty
       c <- PriorityQueue.DeleteMin
       If c is the goal
           Exit
       Else
           Foreach neighbor n of c
               If n "Unvisited"
                   Mark n "Visited"                    
                   Insert( n )
           Mark c "Examined"                    
End procedure
```


如图所示是最佳优先算法的伪代码（此代码仅仅体现了搜寻过程，没有存储节点间父子关系来追溯路径）。最佳优先搜索算法路径规划，主要使用了一个优先队列作开启列表来存储搜寻过程中待检索的节点，并保证每次从权值最小节点的开始搜索；使用关闭列表存储不需要再次检索的列表；使用追溯表存储父子节点的关系，用于追溯生成路径。由于与`A*`算法流程出启发函数不同外，其他步骤基本一致，故算法具体细节参考`A*`算法示例部分。

由于在最佳优先搜索算法的贪心策略中并有考虑到从起始点到当前节点已花费的代价，最佳优先搜索找到的很有可能不是最短（代价最小）路径。如下图所示，对于相同的地图分别采用迪杰斯特拉算法和最佳优先搜索算法得到的路径。
使用迪杰斯特拉算法：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563196426421-1563196426427.png)

</div>

使用最佳优先搜索算法：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563196724479-1563196724484.png)

</div>

显然使用最佳优先搜索算法并没有得到最短路径，同时也可以看出最佳优先搜索算法确实大大缩小了搜索范围，大大缩小了算法的时间和空间复杂度。

### 2.3 A*算法

A*算法流程和最佳优先搜索算法类似，只是换了一个启发函数。

A*算法的启发函数比最佳优先搜索算法增加了起始点到当前节点已经花费的代价，来计算节点的评估值：

``f(n) = g(n) + h(n)``

其中，g(n)为初始节点到节点n的实际代价（通过计算可确定），h(n)是从节点n到目标节点的估计代价。

显然：如果`h(n)=0`,只有g(n)作用，则A*算法退化为迪杰斯特拉算法；如果`g(n)=0`,只有h(n)作用，则A*算法退化为最佳优先搜索算法。

#### 2.3.1 A*算法示例

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563198485078-1563198485082.png)

</div>

算法中相关定义：

- **start**：路径规划的起始点，即上图A位置；
- **goal**：路径规划的目标点，即上图B位置；
- **g_score**：启发式中g(n)部分，当前点到沿着start点A产生的路径到A点的移动耗费；
- **h_score**：启发式中h(n)部分，不考虑不可通过区域，当前点到goal点B的理论移动耗费；
- **f_score**:g_score+h_score，通常也写为F=G+H;
- **开启列表openset**：寻路过程中的待检索节点列表;
- **关闭列表closeset**：不需要再次检索的节点列表;
- **追溯表comaFrom**：存储父子节点关系的列表，用于追溯生成路径。

**1)开始搜索**：

- 搜索的开始从A点开始，首先将A点加入开启列表，初始阶段开启列表中只有A一个节点，因此将A点从开启列表中取出，将A点加入关闭列表。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563199798722-1563199798728.png)

</div>

- 取出A点的相邻点，将相邻点加入开启列表。如图所示，此时A点即为相邻点的父节点。图中箭头指向父节点。将相邻点与A点加入追溯表中。

**2)计算启发函数值**：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563199982355-1563199982359.png)

</div>

对相邻点，依次计算每一点的g_score，h_score，最后得到f_score，如上图所示。

**3)选取最小值再次进行搜索**：

- 选出开启列表中的F值最小的节点，将此节点设为当前节点，移出开启列表，同时加入关闭列表。


<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563200414175-1563200414181.png)

</div>

- 取出当前点的相邻点，当相邻点为关闭点或为障碍时，不操作。此外，查看相邻点是否在开启列表中，如不在开启列表中将相邻点加入开启列表。如相邻点已经在开启列表中，则需要进行G值判定。

**4)G值判定**：

对于相邻点在开启列表中的，计算相邻点的G值，计算按照当前路径的G值与原开启列表中的G值大小。
- 如果当前路径G值小于原开启列表G值，则相邻点以当前点为父节点，将相邻点与当前点加入追溯表中，同时更新此相邻点的G值;
- 如果当前路径G值大于等于原开启列表G值，则相邻点按照原开启列表中的节点关系，G值不变。

**5)重复步骤2、3、4**：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563201513254-1563201513259.png)

</div>

- 直到当goal加入到开启列表中，则完成搜索，在追溯表中回溯父节点得到路径；
- 若没有找到goal点，同时开启列表已为空时，则搜索不到路径，结束搜索。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/1563201838719-1563201838726.png)

</div>


#### 2.3.2 A*算法更快还是更准确

要得到一条行得通好的路径即可，而不一定要找到最短路径。这时可以增加启发式函数h(n)的比重，从而使得A*算法更快。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/Astar_Dijkstra2.gif)

</div>

同样，若要得到一条更短的路径，忽略搜寻范围和耗费时间，则可以增加启发式函数g(n)的比重，从而使得A*算法寻求路径更短。

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/15/Astar_Dijkstra1.gif)

</div>

### 2.4 其他算法

继A*算法之后，还有诸多新算法的提出，如D*算法（又称动态A*算法），该算法遇到动态环境时无需重新规划，但是针对环境复杂的发区域地图，存储量会激增，且动态障碍频繁出现时任需要重新规划。还有诸多智能生物算法，如遗传算法模拟进化论观点，执行效率较高；缺点在于占用内存空间大，个体编码方式和初始种群的确定难度高。

## 3. A*算法的改进

### 3.1 传统A*算法的不足

- 传统A*算法容易忽略机器人实际大小，将其看作质点，只有障碍物所在节点不可通行，其相邻节点均在可通行范围之内。在面向移动机器人实际应用场景下，路径可能紧贴障碍物，容易发生碰撞；
- 在面向机器人室内多U型障碍的特殊场景下规划路径时,算法的效率不高。

### 3.2 改进障碍搜索方式

引入一种基于邻域矩阵的障碍搜索方式，在实际应用中需要根据机器人的自身尺寸及地图分辨率选择合适尺寸的邻域搜索矩阵，来确定与障碍物的间距，保证机器人运行过程中的安全问题。邻域矩阵对路径规划的影响主要与类型和尺寸有关。

### 3.2.1 邻域矩阵类型的选择

邻域矩阵以矩阵中1的个数命名。以如下图所示的12领域搜索矩阵（S~12~）为例，

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/16/1563244158397-1563244158403.png)

</div>

判断当前节点是否可通行的依据是：矩阵中心点（3,3）对应当前节点，此时标识为1的位置对应的节点若均为自由节点（非障碍），则认为当前节点可通行; 否则则不可通行。

根据邻域搜索方向分布可以分为十字搜索和米子搜索两类：

- 大小为5*5的米子邻域搜索矩阵S16：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/16/1563245216746-1563245216752.png)

</div>

- 大小为3*3的十字型搜索矩阵S4：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/16/1563245551236-1563245551241.png)

</div>

采用相同的地图环境，分别使用不同大小和类型的几种邻域矩阵进行路径搜寻：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/16/1563248418930-1563248418935.png)

</div>

如上图所示是两类方法规划路径的对比。可以看到在矩阵大小相同的情况下，从定性地角度分析，两种方式的搜索范围，路径走向以及与障碍物之间的最小间隔都基本一致。下表是对相同矩阵大小下对两种不同搜索方式的定量分析：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/16/1563247869394-1563247869403.png)

</div>

总得看来，十字障碍搜索比米子效率更高，规划路径更优。

### 3.2.2 领域矩阵大小的选择

邻域矩阵的大小决定了障碍物与路径之间的间隔，对计算效率也有影响。如下图所示为不同大小邻域矩阵搜索障碍物获得的路径：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/16/1563248933310-1563248933314.png)

</div>

可以看到随着邻域矩阵的增大，规划路径与障碍物的最小间隔增大。另一方面的考虑，随着邻域矩阵的增大，可达到与地图建模是“障碍膨胀”相似的效果。因而搜索领域矩阵越大，认为不可通行的节点会越多相应地减少了需要搜索的节点数，总体上提高了规划速度。但与此同时，为了远绕障碍，规划路径的长度会有所增加。如下图所示，量化了路径长度、搜索时间与邻域变化的关系：

<div align = "center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/16/1563249506988-1563249506993.png)

</div>

可以看到邻域增大对算法确实有双向硬性，一方面提高搜索效率，但另一方面会是路径长度增加。在应用中，需要结合机器人的尺寸以及室内地图的分辨率，计算出安全间隔对应的栅格数，然后选择一个能满足安全性前提且路径较短的领域矩阵。

3.2 改进启发函数

结合距离信息和角度信息的启发函数：


