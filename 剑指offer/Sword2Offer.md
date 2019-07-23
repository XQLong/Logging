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

### 代码示例


 