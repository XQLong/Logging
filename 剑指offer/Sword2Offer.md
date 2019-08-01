# 剑指offer题解

按照牛客网剑指offer题库的顺序，刷题相关笔记记录如下，详细代码可以参考	[XQL剑指offer笔记](https://github.com/XQLong/java_workplace/tree/master/src/Sword2Offer)。

（注：题目所用到的二叉树、链表、复杂链表等相关类放在TestInstance文件夹，测试用例写在Solution类中）

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

- ==利用`n = n&(n-1)`==

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
- 方法2：创建一个判断当前节点到末节点大小是否为k的函数，将原问题递归转化为子问题进行求解。

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

<div align="center">

![reverseList2](https://github.com/XQLong/java_workplace/blob/master/img/ReverseList2.png)

</div>

- 递归法转换为子问题的求解，例如对于长度为n的链表转换为第一个头结点head和后面n-1个节点的链接反转问题，依次类推递归求解：

<div align="center">

![reverseList1](https://github.com/XQLong/java_workplace/blob/master/img/ReverseList1.png)

</div>


### 代码示例

- 头插法：

```
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode newHead = new ListNode(-1);
        ListNode cur = head;
        while(cur!=null){
            ListNode left = newHead.next;
            newHead.next = cur;
            cur = cur.next;
            newHead.next.next = left;
        }
        return newHead.next;
    }
}
```

- ==递归法==

```
public class Solution {
    public ListNode ReverseList(ListNode head) {
        if(head==null||head.next==null) return head;
        ListNode next = head.next;
        head.next = null;
        ListNode child = ReverseList(next);
        next.next = head;
        return child;
    }
}
```

## 16、合并两个排序的链表

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

### 解答思路

- 使用两个指针分别指向两个链表，比较当前位置两个值的大小，从而选取节点进行连接操作，更新指针位置和新链表尾结点，直至两个链表的节点遍历完。

<div align="center">

![MergeLinkList1](https://github.com/XQLong/java_workplace/blob/master/img/MergeLinkList1.png)

</div>

- 递归转化为子问题的求解，若list1的第一个节点小于list2的第一个节点，则合并后的链表头结点为list1，那么问题转为求取list1.next和list2合并的问题，依次递归；反过来list1的第一个节点大于list2的第一个节点，则合并后的链表头结点为list2，那么问题转为求取list1和list2.next合并的问题,依次递归。

<div align="center">

![MergeLinkList2](https://github.com/XQLong/java_workplace/blob/master/img/MergeLinkList2.png)

</div>


### 代码示例

- 迭代两链表所有节点进行新链表连接：

```
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        ListNode head = new ListNode(-1);
        ListNode p1 = list1;
        ListNode p2 = list2;
        ListNode cur = head;
        while(p1!=null&&p2!=null){
            if(p1.val<p2.val){
                cur.next = p1;
                ListNode next = p1.next;
                p1.next = null;
                cur = p1;
                p1 = next;
            }else{
                cur.next = p2;
                ListNode next = p2.next;
                p2.next = null;
                cur = p2;
                p2 = next;
            }
        }
        if(p1==null&&p2!=null) cur.next = p2;
        if(p1!=null&&p2==null) cur.next = p1;
        return head.next;
    }
}
```

- ==递归转化为子问题==：

```
public class Solution {
    public ListNode  Merge(ListNode list1,ListNode list2) {
        if(list1==null) return list2;
        if(list2==null) return list1;
        if(list1.val<list2.val){
            list1.next = Merge(list1.next,list2);
            return list1;
        }else{
            list2.next = Merge(list1,list2.next);
            return list2;
        }
    }
}
```

## ==17、树的子结构==

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

### 解答思路

使用递归的思路将问题转为子问题的求解，判断从root1当前根节点开始是否包含root2,不包含则转化为root1左右子树的判断，直至搜寻完root1中所有的节点。该算法流程主要由两个函数构成：
- 判断从根节点开始比较，root1是否包root2部分的`isContain`函数；
- 遍历root1中的所有结点作为根节点，分别与子树进行比较的`HasSubtree`函数。

### 代码示例

```
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root2==null) return false;
        if(root1==null) return false;
        if(isContain(root1,root2)) return true;
        return HasSubtree(root1.left,root2)||HasSubtree(root1.right,root2);
    }
    public boolean isContain(TreeNode root1,TreeNode root2){
        if(root2==null) return true;    //root2为子树
        if(root1==null) return false;
        if((root1.val!=root2.val)) return false;
        else return isContain(root1.left,root2.left)&&isContain(root1.right,root2.right);   
    }
}
```

## ==18、二叉树的镜像==

操作给定的二叉树，将其变换为源二叉树的镜像。

### 解答思路

对二叉树进行后续遍历各个节点，依次调换各个节点的左右子树，最终得到二叉树的镜像。

### 代码示例

```
public class Solution {
    public void Mirror(TreeNode root) {
        if(root==null) return;
        Mirror(root.left);
        Mirror(root.right);
        TreeNode left = root.left;
        TreeNode right = root.right;
        root.left = right;
        root.right = left;
    }
}
```

## 19、顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

### 解答思路

先确定矩阵初始的四个顶点，打印一圈后重新确定四个顶点，这样一圈圈打印直至所有位置都打印完。其中要注意当左右边界或上下边界重合时的重复打印的情况。

### 代码示例

```
import java.util.ArrayList;
public class Solution {
    ArrayList<Integer> res = new ArrayList<>();
    public ArrayList<Integer> printMatrix(int [][] matrix) {
        int width = matrix[0].length, height = matrix.length;
        if(width<=0||height<=0) return null;
        //各个边界点的上下左右四个起始值
        int l=0, r=width-1, u=0, d=height-1;
        while(r>=l&&d>=u){
            print(matrix,l,r,d,u);
            l++;
            r--;
            u++;
            d--;
        }
        return res;
    }
    public void print(int [][] matrix,int l,int r,int d,int u){
        //上
        for(int i=l;i<=r;i++){
            res.add(matrix[u][i]);
        }
        //右
        for(int i=u+1;i<d;i++){
            res.add(matrix[i][r]);
        }
        //下
        if(u!=d){
            for(int i=r;i>=l;i--){
                res.add(matrix[d][i]);
            }
        }
        //左
        if(l!=r){
            for(int i=d-1;i>u;i--){
                res.add(matrix[i][l]);
            }
        }
    }
}
```

## 20、包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

### 解答思路

分别用两个栈`stack`来保存数据值，用`min`来保存最小值。
- 当对数据栈`stack`进行入栈操作时，选择`min`中当前最小值与入栈值进行比较确定较小值，入栈`min`。

- 入栈值中较小的压入`min`；
当对数据栈`stack`进行出栈操作时，也对`min`进行出栈操作。

### 代码示例

```
import java.util.Stack;
public class Solution {
    Stack<Integer> stack = new Stack<Integer>();
    Stack<Integer> min = new Stack<Integer>();
    public void push(int node) {
        int mini = !min.isEmpty()?min.peek():node;
        mini = mini<node?mini:node;
        min.push(mini);
        stack.push(node);
    }
    public void pop() {
        stack.pop();
        min.pop();
    }
    public int top() {
        return stack.peek();
    }
    public int min() {
        return min.peek();
    }
}
```

## ==21、栈的压入、弹出序列==

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

### 解答思路

使用栈根据两数组还原入栈、出栈操作。

<div align="center">

![ArrayOfStackPopAndPush](https://github.com/XQLong/java_workplace/blob/master/img/ArrayOfStackPopAndPush.png)

</div>

### 代码示例

```
import java.util.Stack;
public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        Stack<Integer> stack = new Stack<Integer>();
        int ind = 0 ,len=popA.length;
        if(len!=pushA.length) return false;
        for(int i=0;i<len;i++){
            stack.push(pushA[i]);
            while(!stack.isEmpty()&&stack.peek()==popA[ind]){
                stack.pop();
                ind++;
            }
        }
        return stack.isEmpty()?true:false;
    }
}
```

## 22、从上往下打印二叉树

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

### 解答思路

使用一个队列，利用队列先进先出的特点来完成二叉树的层次遍历。

### 代码示例

```
import java.util.ArrayList;
import java.util.Queue;
import java.util.LinkedList;

public class Solution {
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        ArrayList<Integer> res = new ArrayList<>();
        if(root==null) return res;
        queue.add(root);
        while(!queue.isEmpty()){
            TreeNode cur = queue.poll();
            res.add(cur.val);
            if(cur.left!=null) queue.add(cur.left);
            if(cur.right!=null) queue.add(cur.right);
        }
        return res;
    }
}
```

## ==23、二叉搜索树的后续遍历序列==

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

### 解答思路

首先得先二叉搜索树的概念：二叉搜索树也称二叉查找树、有序二叉树或排序二叉树，是指一颗空树或者具有下列性质的二叉树：

- 若任意节点的左子树不为空，则左子树上所有的节点的值均小于它的根节点的值；
- 若任意节点的右子树不为空，则右子树上所有的节点的值均大于它的根节点的值；
- 任意节点的左、右子树也分别是二叉查找树；
- 没有键值相等的节点。

<div align="center">

![BinarySearchTree](https://github.com/XQLong/java_workplace/blob/master/img/BinarySearchTree.png)

</div>

算法思想：首先根据后续遍历的特点找到二叉树的根节点及其左右子树上的节点，验证左右子树是的所有节点都满足与二叉搜索树根节点的大小关系。
对数组进行递归验证所有子树，当所有子树都满足时，则该数组为某二叉搜索树的后续遍历序列。


### 代码示例

```
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        int len = sequence.length;
        if(sequence.length==0) return false;
        return VerifySquence(sequence,0,len-1);
    }
    public boolean VerifySquence(int [] sequence,int l,int r) {
        if(r-l<=1) return true;
        int val = sequence[r];
        int ind = r;
        //求右子树的左边界
        while(ind-1>=l&&val<sequence[ind-1]) ind--; 
        int l2 = ind, r2 = r-1;    //右子树在矩阵中边界
        //判断左子树的左边界是否为l
        while(ind-1>=l&&val>sequence[ind-1]) ind--; 
        if(ind!=l) return false;    //不符合二叉搜索树性质
        int l1 = l, r1 = l2-1;    //左子树在矩阵中边界
        return VerifySquence(sequence,l1,r1)&&VerifySquence(sequence,l2,r2);
    }
}
```

## ==24、二叉树中和为某一值的路径==

输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

### 解答思路

采用回溯法进行路径遍历，将问题递归为子问题，当路径上节点的数值之和大于`target`则返回上一级搜寻，小于`target`则重新确定新的`target`搜寻其左右子树。需要为每个搜寻路径划分空间单独存储：`ArrayList<Integer> array = new ArrayList<>(arr)`。

### 代码示例

```
import java.util.ArrayList;
public class Solution {
    ArrayList<ArrayList<Integer>> res = new ArrayList<>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        ArrayList<Integer> arr = new ArrayList<>();
        findPath(root,target,arr);
        return res;
    }
    public void findPath(TreeNode root,int target,ArrayList<Integer> arr) {
        if(root==null) return;
        ArrayList<Integer> array = new ArrayList<>(arr);
        int value = root.val;
        if(value>target) return;
        else {
            array.add(value);
            if(target==value&&root.left==null&&root.right==null) res.add(array);
            if(root.left!=null) findPath(root.left,target-value,array);
            if(root.right!=null) findPath(root.right,target-value,array);
        }
    }
}
```

## ==25、复杂链表的复制==

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

### 解答思路

本题解答的技巧性较强，算法主要步骤分为以下三步：

<div align="center">

<img src="https://github.com/XQLong/java_workplace/blob/master/img/copyRandomListNode.png"></img>

</div>

### 代码示例

```
public class Solution {
    public RandomListNode Clone(RandomListNode pHead)
    {
        if(pHead==null) return null; 
        insertCopyNode(pHead);
        copyNodeRandom(pHead);
        return breakNode(pHead);
    }
    //在每个节点后面插入复制的节点
    public RandomListNode insertCopyNode(RandomListNode pHead){
        RandomListNode node = new RandomListNode(pHead.label);
        RandomListNode left = pHead.next;
        pHead.next = node;
        if(left!=null) node.next = insertCopyNode(left);
        return pHead;
    }
    //对复制节点的 random 链接进行赋值
    public void copyNodeRandom(RandomListNode pHead){
        RandomListNode cur = pHead;
        RandomListNode curCopy = pHead.next;
        while(cur!=null){
            curCopy.random = cur.random!=null?cur.random.next:null;
            cur = curCopy.next;
            curCopy = cur!=null?cur.next:null;
        }
    }
    //链表拆分
    public RandomListNode breakNode(RandomListNode pHead){
        RandomListNode cur = pHead;
        RandomListNode curCopy = pHead.next;
        RandomListNode res = pHead.next;
        while(cur!=null){
            cur.next = curCopy.next;
            cur = curCopy.next;
            curCopy.next = cur!=null?cur.next:null;
            curCopy = cur!=null?cur.next:null;
        }
        return res;
    }
}
```

## ==26、二叉搜索树与双向链表==

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

### 解答思路

二叉搜索树中序遍历的顺序便是二叉搜索树节点按从小到大的排序顺序。对二叉搜索树进行中序遍历，分别使用leftHead和rightHead记录当前重构的双向链表的左边头和右边头位置，依次进行节点连接。

<div align="center">

    <img src="https://github.com/XQLong/java_workplace/blob/master/img/BSTDoubleLink.png"></img>

</div>                                     

### 代码示例

```
public class Solution {
    TreeNode leftHead;
    TreeNode rightHead;
    public TreeNode Convert(TreeNode pRootOfTree) {
        inorderTraversal(pRootOfTree);
        return leftHead;
    }
    //对二叉树进行中序遍历,形成双向链表
    public void inorderTraversal(TreeNode root){
        if(root==null) return;
        inorderTraversal(root.left);
        if(leftHead==null&&rightHead==null){
            leftHead = root;
            rightHead = root;
        }else{
            root.left = rightHead;
            rightHead.right = root;
            rightHead = root;
        }
        inorderTraversal(root.right);
    }
}
```

## ==27、字符串的排列==

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

### 解答思路
- 可以直接穷举所有可能，使用HashSet的性质来去除重复字符串；
- 可以使用下图所示的递归思想，不断递归使用字符串后的其它字符进行当前字符替换，得到字符组成的所有的字符串组合，每一层迭代使用一个list数组来存储当前交换字符之前已出现的字符，若当前字符存在于list中则跳过，从而避免重复字符串造成的重复结果。在没有重复的情况下：

<div align="center">

    <img src="https://github.com/XQLong/java_workplace/blob/master/img/StringSort.png"></img>

</div>
在有重复时使用`list`来保存同一层递归中前面已经出现的字符：

<div align="center">

    <img src="https://github.com/XQLong/java_workplace/blob/master/img/StringSort1.png"></img>

</div>

其中题目用例测试结果会对返回字符串集合中的字符串先后顺序有要求，在对字符进行替换时，要保证其它字符的相对位置不变。例如对于字符串“abc”现要使用ind=2位置的'c'替换ind=0位置的'a',则替换后为“cab”,a和b的相对位置不变。

### 代码示例

```
import java.util.ArrayList;
public class Solution {
    ArrayList<String> res = new ArrayList<String>();
    public ArrayList<String> Permutation(String str) {
        char[] chars = str.toCharArray();
        int len = chars.length;
        if(len==1){
            res.add(str);
            return res;
        }
        getStringArr(str,0,chars,len);
        return res;
    }
    public void getStringArr(String str,int p,char[] chars,int len){
        if(p==len-1) return;
        ArrayList<String> arr = new ArrayList<>();
        for(int i=p;i<len;i++){
            String s = String.valueOf(str.charAt(i));
            if(!arr.contains(s)) {
                arr.add(s);
                String child = exchange(str,p,i);
                if(p==len-2) res.add(child);
                getStringArr(child,p+1,child.toCharArray(),len);
            }
            
        }
    }
    //字符替换
    public String exchange(String str,int ind1,int ind2){
        if(ind1==ind2) return str;
        String res = "";
        String p1 = str.substring(0,ind1);
        String p2 = str.substring(ind1,ind2);
        String p3 = str.substring(ind2,ind2+1);
        String p4 = str.substring(ind2+1,str.length());
        res = p1+p3+p2+p4;
        return res;
    }
}
```

## 28、数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

### 解答思路

- 方法1：使用HashMap的性质，统计数组中数字出现的个数

- ==方法2：使用`Moore’s Vote`算法：==

`Moore’s Vote`算法比较直观的解释：在数组中找到两个不相同的元素并删除它们，不断重复此过程，直到数组中元素都相同，那么剩下的元素就是主要元素。因为直接在里面删除元素是很费时的，取而代之的可以利用一个计数变量来实现。

```
初始化计数值cnt为0
扫描数组
    如果cnt=0,num确定为数组当前扫描值，cnt=1
    如果cnt!=0
        num与数组当前扫描至相等，则cnt++
        num与数组当前扫描不相等，则cnt--
```

当我们找到这样一个元素时，还要进一步验证一下它是否满足条件。即再遍历一遍，统计它的出现次数。

### 代码示例

- HashMap统计重复

```
import java.util.HashMap;
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int len = array.length;
        if(len==0) return 0;
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        for(int i=0;i<len;i++){
            if(!map.containsKey(array[i])){
                map.put(array[i],1);
                if(1>len/2) return array[0];
            }else{
                int vote = map.get(array[i]);
                vote++;
                if(vote>len/2) return array[i];
                map.put(array[i],vote);
            }
        }
        return 0;
    }
}
```


- Moore Vote方法

```
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int len = array.length;
        if(len==0) return 0;
        int vote =0,num= array[0];
        for(int i=0;i<len;i++){
            if(vote==0){
                num= array[i];
                vote=1;
            }else{
                if(num==array[i]){
                    vote++;
                }else{
                    vote--;
                }
            }
        }
        for (int j=0,count = 0;j<len;j++){
            if(array[j]==num) count++;
            if(count>len/2) return num;
        }
        return 0;
    }
}
```

## 29、最小的K个数

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

### 解答思路

- 最直观的思路，直接对数组进行排序后取前K个值即可（但是题目可能会要求不能使用类库中的排序算法）；
- 使用优先队列PriorityQueue实现一个小顶堆（PriorityQueue简单地理解即为默认将其中元素按从小到大排序的队列）；
- 使用优先队列PriorityQueue维持一个大小为K的大顶堆，过程如下：在添加一个元素之后，如果大顶堆的大小大于 K，那么需要将大顶堆的堆顶元素去除。；
- 利用类似快速排序中对数组进行划分的过程来求取。

（注：常见排序算法的简单分析和详细代码示例，可以参看[排序算法](https://github.com/XQLong/java_workplace/tree/master/src/Algorithms/SortMethod)）

### 代码示例

- 直接排序

```
import java.util.Arrays;
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        int len = input.length;
        if(len<k) return res;
        Arrays.sort(input);
        for(int i=0;i<k;i++){
            res.add(input[i]);
        }
        return res;
    }
}
```

- ==维持大小为K的大顶堆==

```
import java.util.PriorityQueue;
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        int len = input.length;
        if(len<k||k==0) return res;
        PriorityQueue<Integer> queue = new PriorityQueue<Integer>((o1,o2)->(o2-o1));
        for(int i=0;i<len;i++){
            queue.offer(input[i]);
            if(queue.size()>k){
                queue.poll();
            }
        }
        while(!queue.isEmpty()){
            res.add(queue.poll());
        }
        return res;
    }
}
```

- ==使用快速排序思想==

```
import java.util.ArrayList;
public class Solution {
    int [] temp;
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        int len = input.length;
        if(k>len) return res;
        temp = new int[len];
        for(int i=0;i<len;i++) temp[i] = input[i];
        findPartition(0,len-1,k);
        for (int j=0;j<k;j++) res.add(temp[j]);
        return res;
    }
    public void findPartition(int i,int j,int k){
        if(k==temp.length||i>=j) return;
        int mid = partition(i,j);
        if(k==mid) return;
        if(mid>k){
            findPartition(i,mid-1,k);
        }else {
            findPartition(mid+1,j,k);
        }
    }
    public int partition(int i,int j){
        int len = temp.length;
        if(i>=j||i<0||j>=len) return 0;
        int a0 = temp[i],l = i+1,r = j;
        while (r-l>=0){
            while (temp[l]<a0) l++;
            while (temp[r]>=a0) r--;
            if(temp[l]>=a0&&temp[r]<a0){
                temp = swaparr(temp,l,r);
                r--;
                l++;
            }
        }
        temp = swaparr(temp,i,r);
        return r;
    }
    public int[] swaparr(int[] input,int i,int j){
        if(i==j) return input;
        int t = input[i];
        input[i] = input[j];
        input[j] = t;
        return input;
    }
}
```

## 30、连续子数组的最大和

HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)

### 解答思路

利用动态规划思想，已知`0,..,k`的最大和以后，`0,…k+1`的最大和为：

- 若`sum[k]>=0`,`sum[k+1]=sum[k]+A[k+1]`；
- 若`sum[k]<0`,`sum[k+1]=A[k+1]`。

使用`curMax`来保存当前最大连续子数组和,每次`curMax`的求取在上一个`curMax`和`Sum`之间取大者。通俗地讲，如果前K项的累加和Sum<0，加上后面的第K+1项array[k+1]后只会小于array[k+1]，即连续子数组在K项出断裂，故将sum置为0，curMax取当前curMax和array[k+1]中较大的。

### 代码示例

```
import java.lang.Math;
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        int len = array.length;
        int curMax = array[0], Sum = array[0], ind =1;
        while(ind<len){
            if(Sum<0) Sum = 0;
            Sum += array[ind];
            curMax = Math.max(Sum,curMax);
            ind++;
        }
        return curMax;
    }
}
```

## 31、整数中1出现的次数

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

### 解答思路

根据题目记`i`到`j` 之间1出现的次数为`N(i,j)`，可得如下推导：

````
    N(0,9) = 10*0+1;
    N(0,99) = 10N(0,9)+10;
    N(0,999) = 10N(0,99)+100;
    ...
```` 

因而对于任意一个正整数，可利用上市进行拆分后计算。例如整数`1234`：
````
    N(0,1234)=1*N(0,999)+N(1000,1234)
             =1*N(0,999)+(234+1)+N(0,234)
             =1*N(0,999)+(234+1)+N(0,99)+N(100,234)
             =1*N(0,999)+(234+1)+N(0,99)+N(100,199)+N(200,234)
             =1*N(0,999)+(234+1)+2*N(0,99)+100+N(0,34)
             =[1*N(0,999)+1*(234+1)]+[2*N(0,99)+100]+[3*N(0,9)+10]+[4*0+1]
````
由此可以推导出一般结论。又如整数`2345`：
````
    N(0,2345)=[2*N(0,999)+1000]+[3N(0,99)+100]+[4N(0,9)+10]+[5*0+1]
````
因而只需分别计算整数各个位上的部分最后求和，需注意的是当该位上的值不大于1的情况。 

### 代码示例

```
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        int curn = n;
        int p = 1,a = 0,pow = 1,cnt = 0;
        while(curn!=0){
            p = curn%10;
            if(p>1){
                cnt += p*a+pow;
            }else{
                cnt += p*a+p*(n%pow+1);
            }
            a = (10*a+pow);
            pow *= 10;
            curn=curn/10;
        }
        return cnt;
    }
}
```

## 32、把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

### 解答思路

分别将数组中的整数两两组合成的新整数，例如`n1`和`n2`可组合成`n1n2`和`n2n1`，若`n1n2<n2n1`则说明
`n1`应放在`n2`前面，进而统计最小值中每个整数前面的整数个数。具体的如：

````
{3，32，321}
323<332           32在3前面
3213<3321         321在3前面
32132<32321       321在32前面
统计可得{整数：在之前的整数个数}=>[{3：2},{32:1},{321:0}]
于是的到最小整数为：321323
````

通过这种类似投票的途径得到vote[]，vote对应位置的值即为number数组中相应整数的vote值，根据此值确定整数的位置。

### 代码示例

- 自己写的low代码:

```
public class Solution {
    public String PrintMinNumber(int [] numbers) {
        int len = numbers.length;
        int[] vote = new int[len];
        String res = "";
        for(int i=0;i<len;i++){
            for(int j=i+1;j<len;j++){
                if(isLarge(numbers[i],numbers[j])){
                    vote[i]++;
                }else{
                    vote[j]++;
                }
            }
        }
        String[] strs = new String[len];
        for(int i=0;i<len;i++){
            strs[vote[i]] = String.valueOf(numbers[i]);
        }
        for(String s:strs) res += s;
        return res;
    }
    //判断整数a和b组合成的新整数ab和ba的大小，ab>ba返回true;
    public boolean isLarge(int a,int b){
        String sa = String.valueOf(a);
        String sb = String.valueOf(b);
        return Long.valueOf(sa+sb)>Long.valueOf(sb+sa)?true:false;
    }
}
```

- ==更优雅的实现==

```
import java.util.Arrays;
public class Solution {
    public String PrintMinNumber(int[] numbers) {
        if (numbers == null || numbers.length == 0)
            return "";
        int n = numbers.length;
        String[] nums = new String[n];
        for (int i = 0; i < n; i++)
            nums[i] = numbers[i] + "";
        Arrays.sort(nums, (s1, s2) -> (s1 + s2).compareTo(s2 + s1));
        String ret = "";
        for (String str : nums)
            ret += str;
        return ret;
    }
}
```

重写了Arrays类的排序方法。

## 33、丑数

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

### 解答思路

解题思路为如果已知了n个丑数，第n+1个丑数必然是前面的某个丑数乘以2，或者乘以3，或者乘以5。
至于是谁，就是都尝试一下，取最小。如下图:

<div align="center">
    <img src="https://github.com/XQLong/java_workplace/blob/master/img/UglyNumber.png"></img>
</div> 

分别使用三个索引值p2、p3、p5来分别记录因子2、3、5所计算到的位置，下次计算直接从此处开始。

### 代码示例

```
import java.lang.Math;
public class Solution {
    public int GetUglyNumber_Solution(int index) {
        if(index==0) return 0;
        int[] arr = new int[index+1];
        arr[1] = 1;
        //定义三个索引值分别记录因子2,3,5的当前搜索索引
        int p2 = 1, p3 = 1, p5 = 1;
        int p = 2;    //当前确定的丑数位置
        while(p<=index){
            arr[p] = Math.min(arr[p2]*2,Math.min(arr[p3]*3,arr[p5]*5));
            if(arr[p]==arr[p2]*2) p2++;
            if(arr[p]==arr[p3]*3) p3++;
            if(arr[p]==arr[p5]*5) p5++;
            p++;
        }
        return arr[index];
    }
}
```

## 34、第一个只出现一次的字符

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

### 解答思路

- 使用整型数组记录每个字符出现的次数；
- 考虑到仅需考虑每个字符出现的`0，1，>1`三种情况，故可以使用三个比特位来存储这些信息，从而减小空间复杂度。

### 代码示例

- 使用一个长度为128的整型数组存储各个字符出现的次数：

```
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        int[] arr = new int[128];
        char[] chars = str.toCharArray();
        for(int i=0;i<chars.length;i++){
            int index = chars[i]-' ';
            arr[index]++;
        }
        for(int i=0;i<chars.length;i++){
            int index = chars[i]-' ';
            if(arr[index]==1) return i;
        }
        return -1;
    }
}
```

- 使用BitSet来存储，降低空间复杂度：

```
import java.util.BitSet;
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        BitSet set1 = new BitSet(128);    //记录是否出现过
        BitSet set2 = new BitSet(128);    //记录是否出现超过一次
        char[] chars = str.toCharArray();
        for(int i=0;i<chars.length;i++){
            int ind = chars[i]- ' ';
            if(!set1.get(ind))  set1.set(ind);
            else set2.set(ind);
        }
        for(int i=0;i<chars.length;i++){
            int ind = chars[i]- ' ';
            if(set1.get(ind)&&!set2.get(ind))  return i;
        }
        return -1;
    }
}
```

## 35、数组中的逆序对

在数组中的两个数字，如果前面一个数字大后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

### 解答思路

使用归并排序算法对数组进行排序在排序过程中计算逆序对个数。需注意的两点：
- 结果是对1000000007取模的输出，cnt需定义为long型，再将结果转为int型返回；
- 逆序对的统计在归并排序的归并过程中统计：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/08/01/1564626600902-1564626601162.png)

</div>

### 代码示例

```
public class Solution {
    long cnt = 0;
    int[] arr;
    public int InversePairs(int [] array) {
        int len = array.length;
        arr = new int[len];
        divide(array,0,len-1);
        long y = 1000000007L;
        return (int)(cnt%y);
    }
    public void divide(int[] array,int l, int r){
        if(l>=r) return;
        int mid = (l+r)/2;
        divide(array,l,mid);
        divide(array,mid+1,r);
        merge(array,l,r,mid);
    }
    public void merge(int[] array,int l, int r, int mid){
        if(mid<l||mid>r) return;
        int ind1 = l ,ind2 = mid+1;
        for(int i=l;i<=r;i++){
            if(ind1>mid) arr[i] = array[ind2++];
            else if(ind2>r) arr[i] = array[ind1++];
            else if(array[ind1]<=array[ind2]) arr[i] = array[ind1++];
            else{
                arr[i] = array[ind2++];
                this.cnt += (mid-ind1+1); 
            } 
        }
        for(int j=l;j<=r;j++) array[j] = arr[j];
    }
}
```

## 36、链表的第一个公共节点

输入两个链表，找出它们的第一个公共结点

### 解答思路

<div align="center">

![commonnode](https://github.com/XQLong/java_workplace/blob/master/img/FirstCommonListNode.png) 

</div> 


设 A 的长度为` a + c`，B 的长度为` b + c`，其中 c 为尾部公共部分长度，可知` a + c + b = b + c + a`。
当访问链表 A 的指针访问到链表尾部时，令它从链表 B 的头部重新开始访问链表 B；
同样地，当访问链表 B 的指针访问到链表尾部时，令它从链表 A 的头部重新开始访问链表 A。这样就能控制访问 A 和 B 两个链表的指针能同时访问到交点。

### 代码示例

```
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode cur1 =  pHead1;
        ListNode cur2 =  pHead2;
        while(cur1!=cur2){
            cur1 = cur1==null?pHead2:cur1.next;
            cur2 = cur2==null?pHead1:cur2.next;
        }
        return cur1;
    }
}
```

## 37、数字在排序数组中出现的次数

### 解答思路

统计一个数字在排序数组中出现的次数。

### 代码示例





