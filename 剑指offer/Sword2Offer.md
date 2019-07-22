# 剑指offer题解

按照牛客网剑指offer题库的顺序，刷题相关笔记记录如下，详细代码可以参考	[XQL剑指offer笔记](https://github.com/XQLong/java_workplace/tree/master/src/Sword2Offer)。

## 1、二维数组查找

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

### 解答思路：

转换思考角度，不能盲目的从矩阵（0,0）位置开始查找，根据题意可以知道对于矩阵中任意某个位置的值，其左边相邻的值小于它，而下边相邻的值大于它，因而可以从右上角位置开始，依次网左下角进行搜寻。

同时需注意的是二维矩阵的长度和高度的求取，以及某个位置值的表示，例如对于二维矩阵arr：
```
列数：arr[0].length；
行数：arr.length
位置（x,y）处的值：arr[y][x]
```

### 代码示例：

```
 public boolean Find(int target, int [][] array) {
        int l = array[0].length;    //列数
        int h = array.length;        //行数
        int x = l-1,y=0;
        while(x>=0&&y<h){
            if(array[y][x]==target) return true;
            else if(array[y][x]>target) x--;
            else y++;
        }
        return false;
    }
```

## 2、替换空格

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

### 解答思路：

这里只给出可以另外开辟

### 代码示例：

```
public String replaceSpace(StringBuffer str) {
    	char[] chars = str.toString().toCharArray();
        String res = "";
        for(int i=0;i<chars.length;i++){
            if(chars[i]==' ') res += "%20";
            else res += String.valueOf(chars[i]);
        }
        return res;
    }
```

 