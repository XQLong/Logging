## 一、传输层

传输层是主机才会有的层次。传输层为应用层提供服务，同时是可以使用网络层的服务。传输层的功能：

- 传输层提供进程和进程之间的逻辑通信
- 复用和分用
- 传输层对收到的报文进行差错检测

传输层有TCP和UDP两种协议：
TCP：可靠，面向连接，时延大，适用于大文件；
UDP：不可靠，无连接，时延小，适用于小文件。

## 二、UDP协议

### 主要特点：
- UDP是无连接的，减少开销和发送数据之前的时延；
- UDP使用最大努力交付，即不保证可靠交付；
- UDP是面向报文的，适合一次性传输少量数据的网络应用。

### UDP首部格式：

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566005566102-1566005566471.png)

### UDP校验（UDP检验和）：

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566005700577-1566005700581.png)

伪首部只会在计算检验和时才出现，不向下也不向上传递。

### 检验过程

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566006081759-1566006081763.png)

在发送端：
- 填充上伪首部
- 全零填充检验和字段
- 全零填充数据部分（UDP数据报要看成雨多4B的字串连接起来）
- 伪首部+首部+数据部分采用二进制反码求和
- 把和求反码填入校验和字段
- 去掉伪首部，发送

在接收端：
- 填上伪首部
- 伪首部+首部+数据部分采用二进制反码求和
- 结果全为1则无差错，否则对其数据报/交给应用层附上出错警告

## 三、TCP协议

### TCP协议的特点：
- TCP是面向连接（虚连接）的传输协议。
- 每一条TCP连接只能有两个端点，每一条TCP连接只能是点对点
- 可靠有序，不丢不重
- TCP提供全双工通信
- TCP面向字节流

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566006703252-1566006703258.png)

### TCP报文段首部格式

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566006780803-1566006780806.png)

- 序号：报文段所发送数据的第一个字节的序号；
- 确认号：期望收到对方下一个报文段的第一个数据字节的序号；
- 数据偏移（首部长度）：TCP报文段的数据起始处距离TCP报文段的起始处有多远，以￥B为单位，即1个数值是4B。
- 6个控制位：紧急位；






