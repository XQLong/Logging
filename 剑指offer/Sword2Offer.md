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

这里只给出可以另外开辟一个新字符串的比较容易的情况，另外一些题目可能要求在原字符串上作替换。

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

## 3、从尾到头打印链表

### 解答思路

本题采用递归的思路较为简单，声明一个全局变量来存储返回结果，使用递归对链表进行遍历将链表中的值按从尾到头存入。

### 代码示例

- 递归法：

```
import java.util.ArrayList;
public class Solution {
    ArrayList<Integer> res = new ArrayList();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        traversal(listNode);
        return res;
    }
    public void traversal(ListNode listNode){
        if(listNode==null) return;
        traversal(listNode.next);
        res.add(listNode.val);
    }
}
```
- 链表头插法构造逆序链表法：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/22/1563805689367-1563805689398.png)

</div>

```
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> res = new ArrayList();
        ListNode head = new ListNode(-1);
        ListNode cur = listNode;
        while(cur!=null){
            ListNode left = head.next;
            head.next = cur;
            cur = cur.next;
            head.next.next = left;
        }
        cur = head.next;
        while(cur!=null){
            res.add(cur.val);
            cur = cur.next;
        }
        return res;
    }
}
```

## 4、重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

### 解答思路

本题也用到了迭代的思想，将问题划分为与之类似的子问题进行求解，以一个例子进行说明。若给出
```
前序遍历数组pre：[1,2,4,7,3,5,6,8]
中序遍历数组in：[4,7,2,1,5,3,8,6]
```
- 根据前序遍历的定义显然二叉树的根节点(记为root)的val为pre[0]=1，那么问题转化为从此根节点从发求取其左右子树的问题。
- 又结合中序遍历的特点，在数组in中值为1的位置的两边分别为其左右字数的值，即左子树部分pre:[2,4,7],in[4,7,2];右子树部分pre:[3,5,6,8],in:[5,3,8,6]。
- 那么求取root的左右节点的问题则归结于求取其子问题，即其左右子树根节点的问题。依次类推，直至递归玩所有的节点则对二叉树完成了重构。
- 具体的编程实现，用到了一个Map来存储in数组的各个值的索引，引入当前问题的前序遍历数组左、右边界prel和prer，当前问题中序遍历数组左、右边界inl和inr，以便用来确定划分为的子问题的在原矩阵中的索引范围。

### 代码示例

```
import java.util.Map;
import java.util.HashMap;
public class Solution {
    //使用一个HashMap来存储in矩阵中各个值的索引
    HashMap<Integer,Integer> index = new HashMap<>();
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        int len = pre.length;
        if(len==0) return null;
        int l = 0,r = len-1;
        for(int i=0;i<len;i++){
            index.put(in[i],i);
        }
        return divide(pre,l,r,l,r);
    }
    //迭代地划分为子问题求解
    public TreeNode divide(int [] pre,int prel,int prer,int inl,int inr){
        if(prel>prer) return null;
        int val = pre[prel];
        int ind = index.get(val);
        int lsize = ind-inl,rsize = inr-ind;
        TreeNode root = new TreeNode(val);
        root.left = divide(pre,prel+1,prel+lsize,inl,ind-1);
        root.right = divide(pre,prel+lsize+1,prel+lsize+rsize,ind+1,inr);
        return root;
    } 
}
```

## 5、两个栈实现队列

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

### 解答思路

用stark1存储push()的值，当进行pop()操作时将stark1中的值先入栈stark2（若stark2为空），再对stark2进行pop()操作。要特别注意只有stark2为空时，才进行上述操作，否则直接对stark2进行pop()。

````
    push(5);
    push(2);
    pop();
    push(0);
    pop();
    pop();
````
<div align="center">

![image](https://github.com/XQLong/java_workplace/blob/master/img/stark_queue.png)

</div>

### 代码示例

```
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    public void push(int node) {
        stack1.push(node);
    }
    public int pop() {
        if(stack2.isEmpty()){
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
```

## 6、旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

### 解答思路

考察结合题目已知信息，利用利用二分查找来优化查找算法。由题意可知，若当区间的左边界和有边界的值不相等时，则必有左边界的值大于等于有边界的值（否则数组没有进行旋转）：
- 若左右边界值相等则无法判断目标值所在区间，无法进行二分查找，回归原始的查找方法；
- 若左边界值大于右边界值，则存在如下图所示两种情况：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/23/1563891575760-1563891576056.png)

</div>

根据l,mid,r三个位置值的大小来确定目标值所在的区间，依次类推。

### 代码示例

```
public class MinNumberInRotateArray {
    public int minNumberInRotateArray(int [] array) {
        int len = array.length;
        if(len==0) return 0;
        return binarySearch(array,0,len-1);
    }
    public int binarySearch(int[] arr,int l,int r){
        if(r-l==1) return arr[l]<arr[r]?arr[l]:arr[r];
        int mid = (l+r)/2;
        if(arr[l]<arr[r]) return arr[l];
        else if(arr[l]>arr[r]) {
            if(arr[l]>arr[mid]){
                return binarySearch(arr,l,mid);
            }else{
                return binarySearch(arr,mid,r);
            }
        }
        else return Search(arr,l,r);
    }
    public int Search(int[] arr,int l,int r){
        int res = arr[l];
        for(int i=l;i<=r;i++){
            if(arr[i]<res) res = arr[i];
        }
        return res;
    }
}
```

## 7、斐波那契数列

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0，n<=39）。

### 解答思路

- 使用递归计算，会造成重复计算一些子问题。

例如，计算 f(10) 需要计算 f(9) 和 f(8)，计算 f(9) 需要计算 f(8) 和 f(7)，可以看到 f(8) 被重复计算了。

- 动态规划。也是将一个问题划分为多个子问题进行求解，但动态规划会把子问题缓存起来，从而避免重复求解子问题。

- 针对上述方法的两种改进：

因为考虑到第i项只与第i-1和i-2项相关，因此只需存储前两项的值，从而将空间复杂度由O(N)降低到O(1)。

由于待求解的n小于40，因此可以将前 40 项的结果先进行计算，之后就能以 O(1) 时间复杂度得到第 n 项的值了。

### 代码示例

- 动态规划

```
public class Solution {
    public int Fibonacci(int n) {
        if(n<=1) return n;
        int[] arr = new int[n+1];
        arr[0] = 0;
        arr[1] = 1;
        for(int i=2;i<=n;i++){
            arr[i] = arr[i-1]+arr[i-2];
        }
        return arr[n];
    }
}
```

- 动态规划的优化（仅存储两个值）

```
public class Solution {
    public int fibonacci(int n) {
        if (n<=1) return n;
        int pre1 = 1;
        int pre2 = 0;
        int i=1;
        int res = 0;
        while(i!=n){
            res = pre1 + pre2;
            pre2 = pre1;
            pre1 = res;
            i++;
        }
        return res;
    }	
}
```

## 8、跳台阶

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

### 解答思路

青蛙跳台阶问题：当青蛙即将跳上n级台阶时，共有两种可能性，一种是从n-1级台阶跳一步到n级，另外一种是从n-2级台阶跳两步到n级，所以求到n级台阶的所有可能性`f(n)`就转变为了求到n-2级台阶的所有可能性`f(n-2)`和到n-1级台阶的所有可能性`f(n-1)`之和，以此类推至最后`f(2)=f(0)+f(1)=1+1`。

递推公式就是

``
f(n) = f(n - 1) + f(n - 2)
``

### 代码示例

```
public class Solution {
    public int JumpFloor(int target) {
        if(target<=2) return target;
        int a0 = 1, a1 = 2;
        int res = 0;
        for(int i=3;i<=target;i++){
            res = a0+a1;
            a0 = a1;
            a1 = res;
        }
        return res;
    }
}
```

## 9、变态跳台阶

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

### 解答思路

与上题类似，对于可以跳任意n阶的青蛙（变态青蛙跳问题），则递推公式变为：

``
f(n) = f(n - 1) + f(n - 2)+···+f(1)+1
``
需注意的要加上一次直接跳n阶的情况。

### 代码示例

```
public class Solution {
    public int JumpFloorII(int target) {
        if(target<=2) return target;
        int[] arr = new int[target+1];
        arr[1] = 1;
        arr[2] = 2;
        for(int i=3;i<=target;i++){
            for(int j=1;j<i;j++){
                arr[i] += arr[j];
            }
            //加上直接跳i阶台阶的一种情况
            arr[i]++;
        }
        return arr[target];
    }
}
```

## 10、矩形覆盖

我们可以用`2*1`的小矩形横着或者竖着去覆盖更大的矩形。请问用n个`2*1`的小矩形无重叠地覆盖一个`2*n`的大矩形，总共有多少种方法？

### 解答思路

<div align="center">
    <img src="https://github.com/XQLong/java_workplace/blob/master/img/RectangleCover.png"></img>
</div>

那么对于2*n大小的矩阵可以分为两种情况如上图：

- 第一块竖着放，则剩下的部分覆盖方法数为f(n-1)；
- 第二种第一块横着放，则余下部分覆盖方法为f(n-2)。

所以在n大于2时，可以推导出f(n) = f(n-1)+f(n-2)。

### 代码示例

```
public class Solution {
    public int RectCover(int target) {
        if(target<=2) return target;
        int a1 = 1, a2 = 2;
        int res = 0;
        for(int i=3;i<=target;i++){
            res = a1 +a2;
            a1 = a2;
            a2 = res;
        }
        return res;
    }
}
```

## 11、二进制中1的个数

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

### 解答思路

本题此处主要考虑如下三种方法：

- 对整数进行逐位无符号右移运算
- 通过计算`n = n&(n-1)`依次获取含1的位，直至`n=0`。其中`n = n&(n-1)`的作用是将n的二进制中最低位为1的值置为0。
- 利用java中的Integer类的bitCount函数

### 代码示例

- 无符号右移运算

```
public class Solution {
    public int NumberOf1(int n) {
        int a = 1;
        int res = 0;
        while(n!=0){
            if((n&a)==1) res++;
            n = n>>>1;
        }
        return res;
    }
}
```

- 利用`n = n&(n-1)`

```
public class Solution {
    public int NumberOf1(int n) {
        int res = 0;
        while(n!=0){
            res++;
            n = n&(n-1);
        }
        return res;
    }
}
```

- Integer类的bitCount函数

```
public class Solution {
    public int NumberOf1(int n) {
        return Integer.bitCount(n);
    }
}
```

## 12、数值的整数次方

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

### 解答思路

若题目要求不能使用Math类中的相关计算次方的函数，则直接根据次方的定义利用乘法计算，其中需注意exponent的符号。

### 代码示例

```
public class Solution {
    public double Power(double base, int exponent) {
        if(exponent==0) return 1;
        double b =base;
        int e = exponent<0?(-exponent):exponent;
        while(e>1){
            base *= b;
            e--;
        }
        return exponent<0?(1/base):base;
   }
}
```

## 13、调整数组顺序使奇数位于偶数之前

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

### 解答思路

首先复制数组，计算数组中基数个数，然后重新按就对数组进行排序。

### 代码示例

```
public class Solution {
    public void reOrderArray(int [] array) {
        int len = array.length;
        int[] temp = new int[len];
        int  num = 0;    //统计数组中奇数个数
        for(int i=0;i<len;i++){
            if(array[i]%2!=0) num++;
            temp[i] = array[i];
        } 
        int l = 0, r = l+num;
        for(int j=0;j<len;j++) {
            if(temp[j]%2 != 0){
                array[l] = temp[j];
                l++;
            }else{
                array[r] = temp[j];
                r++;
            }
        }
    }
}
```

## 14、链表中的倒数第K个节点

输入一个链表，输出该链表中倒数第k个结点。

### 解答思路

- 方法1：计算链表长度N，依次进行移动到第N-K个节点处，即为倒数第K个节点。
- 方法2：创建一个判断当前节点到末节点大小是否为k的函数，将原问题迭代转化为子问题进行求解。

### 代码示例

- 方法2：

```
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        if(head==null) return null;
        if(isSizeofK(head,k)) return head;
        return FindKthToTail(head.next,k);
    }
    public boolean isSizeofK(ListNode head,int k){
        int cnt = k;
        ListNode cur = head;
        while(cur!=null){
            k--;
            cur = cur.next;
        }
        return k==0?true:false;
    }
}
```

## 15、反转链表

输入一个链表，反转链表后，输出新链表的表头。

### 解答思路

- 头插法反转链表，参照第三题解答。

### 代码示例

