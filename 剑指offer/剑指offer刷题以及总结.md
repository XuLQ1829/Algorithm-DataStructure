
## 剑指offer题型总结
### 数据结构类题目
| 类型 | 题号 |
| --- | --- |
| 栈和队列 | 09、30、31、59-1、59-2  |
| 链表 | 06、18、22、23、24、25、35、52  |
| 树 | 07、08、26、27、28、32、33、34、36、37、54、55、68  |
| 数组和矩阵 | 03-1、03-2、04、11、17、29、39、42、57-1、57-2   |
| 堆 | 40  |
| 哈希表 | 50、50-2  |
| 图 | 12、13  |

### 具体算法类题目
| 类型 | 题号 |
| --- | --- |
| 递归和动态规划 | 10、10-x、14、24、47、48  |
| 搜索算法 | 04、11、53  |
| 字符串 | 05、19、20、38、46、58-1、58-2、67  |
| 回溯法 | 12、13  |
| 排序算法 | 15、21、40、41、51  |
| 位运算 | 56、56-2、65  |
| 其它算法 | 16、43、44、45、49、60、61、62、63、64、65  |

注：如果能在每个序号上加上对应的超链接就方便了。

## 剑指offer中重要知识点梳理
### 二进制位运算的用法  
1 总结几个位运算的用法，下面举的例子中，a=1010 1010
按位与&  

+ 清零：a&0=0
+ 取指定位上的数字，如取得数字a的最后四位：a&0000 1111 = 0000 1010

按位或|  
+ 对某些位置置为1，如将a的后四位置为1：a|0000 1111 = 1010 1111

异或^  
+ 将某些位置取反，如将a的后四位取反：a^0000 1111 = 1010 0101
+ 与0异或保留原值，如：a^0000 0000 =1010 1010
+ 交换两个的变量值：A=A^B; B=A^B; A=A^B; 可以完成A和B的交换。

口诀：  
清零取数要用与，某位置一可用或。  
若要取反和交换，轻轻松松用异或。  
参考资料：[二进制位运算的几个用法](https://www.cnblogs.com/yongh/p/9971520.html)  

2 **负数右移还是负数！**即如果对n=0x8000 0000右移，最高位的1是不会变的。如果面试题15通过令n=n>>1来计算n中1的个数，该数最终会变成0xFFFF FFFF而陷入死循环！  

3 **把一个整数减1，再和原来的整数做与运算，会把该整数最右边的1变成0**。这种方法一定要牢牢记住，很多情况下都可能用到，例如：  
+ 一句话判断一个整数是否为2的整数次方；
+ 对两个整数m和n，计算需要改变m二进制表示中的几位才能得到n。

4 与数字操作有关的题目，测试时注意边界值的问题。对于32位数字，其正数的边界值为1、0x7FFFFFFF，负数的边界值为0x80000000、0xFFFFFFFF。  

5 当一个数字出现两次（或者偶数次）时，用异或^ 可以进行消除。**一定要牢记异或的这个功能！**（面试题56）  

6 无符号数和有符号的运算  
数值在计算机中是用补码来表示的：
```
[+1] = [00000001]原 = [00000001]反 = [00000001]补
[-1] = [10000001]原 = [11111110]反 = [11111111]补
```
所以计算机的加减法都是补码的加减法，比如：
```
 1-1 = 1 + (-1) = [0000 0001]原 + [1000 0001]原 = [0000 0001]补 + [1111 1111]补 = [0000 0000]补=[0000 0000]原
 
 (-1) + (-127) = [1000 0001]原 + [1111 1111]原 = [1111 1111]补 + [1000 0001]补 = [1000 0000]补
```

参考资料：
《剑指offer》  
[牛客网的剑指offer专题](https://www.nowcoder.com/ta/coding-interviews)    
[LeetCode的剑指offer专题](https://leetcode-cn.com/problemset/lcof/)  
[《剑指Offer》Java实现](https://www.cnblogs.com/yongh/p/9637260.html)  

------
## 03-1  找出数组中重复的数字
题目：在一个长度为n的数组里的所有数字都在0到n-1的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。例如，如果输入长度为7的数组{2, 3, 1, 0, 2, 5, 3}，那么对应的输出是重复的数字2或者3。  

第一种思路：排序，然后找重复数字。时间复杂度：O(nlogn)
```Java
import java.util.Arrays;
public class Solution {
    // 参数说明：
    // numbers：整型数组。
    // length：整型数组的长度
    // duplication：用于存放数组中的任一重复数字，长度为1
    // 返回值说明：
    // 存在重复数字返回true，否则返回false。
    public boolean duplicate(int[] numbers, int length, int[] duplication) {
        if (numbers == null || length <= 0) {
            duplication[0] = -1;
            return false;
        }
        Arrays.sort(numbers);
        for (int i = 1; i < length; i++) {
            if (numbers[i] == numbers[i-1]) {
                duplication[0] = numbers[i];
                return true;
            }
        }
        return false;
    }
}
```
第二种思路：从哈希表的思路拓展，重排数组：把扫描的每个数字（如数字m）放到其对应下标（m下标）的位置上，若同一位置有重复，则说明该数字重复。  
时间复杂度：O(n)
```Java
public class Solution {
    public boolean duplicate(int[] numbers,int length, int[] duplication) {
        // 检查输入合法性
        if (numbers == null || length <= 0) {
            return false;
        }
        for (int num : numbers) {
            if (num < 0 || num >= length) {
                return false;
            }
        }
        
        for (int i = 0; i < length; i++) {
            while (numbers[i]!=i) {
                if (numbers[numbers[i]] == numbers[i]) {
                    duplication[0] = numbers[i];
                    return true;
                }
                // 将i位置上的元素放到numbers[i]的位置上
                int temp = numbers[i];
                numbers[i] = numbers[temp];
                numbers[temp] = temp;
            }
        }
        return false;
    }
}
```
## 03-2  不修改数组找出重复的数字
**题目**：在一个长度为n+1的数组里的所有数字都在1到n的范围内，所以数组中至少有一个数字是重复的。请找出数组中任意一个重复的数字，**但不能修改输入的数组**。例如，如果输入长度为8的数组{2, 3, 5, 4, 3, 2, 6, 7}，那么对应的输出是重复的数字2或者3。  
**思路**：数组长度为n+1，而数字只从1到n，说明必定有重复数字。可以由**二分查找法拓展**：把1~n的数字从中间数字m分成两部分，若前一半1~m的数字数目超过m个，说明重复数字在前一半区间，否则，在后半区间m+1~n。每次在区间中都一分为二，知道找到重复数字。  
```Java
public class FindDuplication2 {
    public int getDuplicate(int[] arr) {
        if (arr == null || arr.length <= 0) {
            System.out.println("Input is illegal.");
            return -1;
        }
        for (int a : arr) {
            if (a < 1 || a > arr.length-1) {
                System.out.println("Input is illegal.");
                return -1;
            }
        }

        int left = 1;
        int right = arr.length-1;
        int mid, count;
        while (left <= right) {
            mid = left + ((right - left)>>1);// 注意运算符的优先级
            count = getCount(arr, left, mid);

            // 循环终止条件
            if (left == right) {
                if (count > 1) {
                    return left;
                }else {
                    break;
                }
            }

            if (count > mid - left + 1) {// 说明重复元素在[left, mid]之间
                right = mid;
            }else {// 说明重复元素在[mid+1, right]之间
                left = mid+1;
            }
        }
        return -1;
    }

    // arr数组中，[left, right] 之间的元素个数
    private int getCount(int[] arr, int left, int right) {
        if (arr == null) {
            return 0;
        }
        int count = 0;
        for (int a : arr) {
            if (a >= left && a <= right) {
                count++;
            }
        }
        return count;
    }
}
```

## 04  二维数组中的查找
第一版：暴力解法，两层for循环。这种解法没有用好每一行递增，每一列递增这个条件。
```Java
public class Solution {
    public boolean Find(int target, int [][] array) {
        if (array.length == 0) {
            return false;
        }
        int n = array.length;
        int m = array[0].length;
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if (array[i][j] < target) {
                    continue;
                }else if (array[i][j] > target) {
                    break;
                }else {
                    return true;
                }
            }
        }
        return false;
    }
}
```
第二版：对于左下角的元素来说，向上数字递减，向右数字递增。因此从左下角开始查找，当target比元素大时右移，target比元素小时上移。右上角也可以方法类似。原理类似**二分查找**，通过和target进行比较，直接一次性排除掉一行或列的数据。
```Java
public class Solution {
    public boolean Find(int target, int [][] array) {
        if (array.length == 0) {
            return false;
        }
        int n = array.length;
        int m = array[0].length;
        int i = n-1;
        int j = 0;
        while (i >= 0 && j < m) {
            if (target < array[i][j]) {
                i--;
            }else if (target > array[i][j]) {
                j++;
            }else {
                return true;
            }
        }
        return false;
    }
}
```

## 05 替换空格
请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。  
思路：**原地从后往前复制**，比从前往后复制的次数少，因此效率更高。先计算出需要的总长度，然后从后往前进行复制和替换，，则每个字符只需要复制一次即可。时间效率为O(n)。  

```Java
public class Solution {
    public String replaceSpace(StringBuffer str) {
    	if (str == null) {
            return null;
        }
        int length = str.length();
        int orgIndex = length-1; //原字符串最后一个位置的索引
        for (int i = 0; i <= orgIndex; i++) {
            if (str.charAt(i) == ' ') {
                //如果是空格，长度加2，因为原本已经有一个空位
                length += 2;
            }
        }
        str.setLength(length); //设置字符串新的长度。
        int newIndex = length-1; //新字符串最后一个位置的索引
        //两个索引从后往前，若newIndex追上orgIndex，说明空格已经填充完毕
        while (newIndex > orgIndex) {
            if (str.charAt(orgIndex) != ' ') {
                str.setCharAt(newIndex--, str.charAt(orgIndex));
            }else {
                str.setCharAt(newIndex--, '0');
                str.setCharAt(newIndex--, '2');
                str.setCharAt(newIndex--, '%');
            }
            orgIndex--;
        }
        return str.toString();
    }
}
```
## 06 从尾到头打印链表
第一版：反转然后遍历链表。思路：先将链表反转，再将链表中各个节点的值加到ArrayList中。但是这种方法**改变了链表的结构**。  
```Java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> res = new ArrayList<>();
        if (listNode == null) {
            return res;
        }
        //将链表反转
        ListNode prev = null, next = null;
        ListNode curr = listNode;
        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        ListNode newHead = prev; //获得反转后链表的头结点
        while (newHead != null) {
            res.add(newHead.val);
            newHead = newHead.next;
        }
        return res;
    }
}
```
第二版：使用栈结构。
```Java
import java.util.ArrayList;
import java.util.Stack;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> res = new ArrayList<>();
        Stack<Integer> stack = new Stack<>();
        while (listNode != null) {
            stack.push(listNode.val);
            listNode = listNode.next;
        }
        while (!stack.isEmpty()) {
            res.add(stack.pop());
        }
        return res;
    }
}
```
第三版：使用递归实现。因为递归本质上就是一个栈结构。
```Java
import java.util.ArrayList;
public class Solution {
    ArrayList<Integer> res = new ArrayList<>();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if (listNode != null) {
            this.printListFromTailToHead(listNode.next); //理解好这句代码为什么不用赋值操作
            res.add(listNode.val);
        }
        return res;
    }
}
```
收获：对于“后进先出”的问题，要快速想到用栈和递归的方法来解决。对于递归，返回的函数值不一定要有赋值操作，只要实现了遍历的作用就可以了。

## 07 重建二叉树
**题目**：输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1, 2, 4, 7, 3, 5, 6, 8}和中序遍历序列{4, 7, 2, 1, 5, 3, 8, 6}，则重建出其二叉树并输出它的头结点。  
**思路**：前序遍历第一个值就是根结点的值，根据该值**在中序遍历中的位置**，可以轻松找出该根结点**左右子树的前序遍历和中序遍历**，之后又可以用同样方法构建左右子树，所以该题可以采用**递归**的方法完成。  
**注意**：在递归问题中，代码可以用下标表示的就用下标表示，不用重新构建新的数组。  

```Java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        TreeNode root = reConstructBinaryTree(pre, 0, pre.length-1, in, 0, in.length-1);
        return root;
    }
    //pre、in：前序遍历数组和中序遍历数组
    //startPre、endPre：前序遍历数组的开始位置和结束位置
    //startIn、endIn：中序遍历数组的开始位置和结束位置
    private TreeNode reConstructBinaryTree(int[] pre, int startPre, int endPre, int[] in, int startIn, int endIn) {
        if (startPre > endPre || startIn > endIn) {
            return null;
        }
        //当前树的根节点
        TreeNode root = new TreeNode(pre[startPre]);
        //遍历找到当前根节点在中序数组中的位置
        for (int i = startIn; i <= endIn; i++) {
            if (in[i] == pre[startPre]) {
                root.left = reConstructBinaryTree(pre, startPre+1, startPre+i-startIn, in, startIn, i-1);
                root.right = reConstructBinaryTree(pre, startPre+i-startIn+1, endPre, in, i+1, endIn);
                break;
            }
        }
        return root;
    }
}
```
## 08 二叉树的下一个节点
**题目**：给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。
**思路**：首先自己在草稿纸上画图，进行分析（不再展开）。可以发现下一个结点的规律为：
1. 若当前结点有右子树时，其下一个结点为右子树中最左子结点；
2. 若当前结点无右子树时：
	+ (1) 若当前结点为其父结点的左子结点时，其下一个结点为其父结点；
	+ (2) 若当前结点为其父结点的右子结点时，继续向上遍历父结点的父结点，直到找到一个结点是其父结点的左子结点（与（1）中判断相同），该结点即为下一结点。
```Java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode){
        if (pNode.right != null) {
            TreeLinkNode curNode = pNode.right;
            while (curNode.left != null) {
                curNode = curNode.left;
            }
            return curNode;
        }else {// pNode的右子树为空，向上找未被遍历的父节点
            TreeLinkNode curNode = pNode;
            while (curNode.next != null) {
                if (curNode.next.left == curNode) {// pNode是它的父节点的左子树
                    return curNode.next;
                }
                curNode = curNode.next;
            }
            return null;
        }
    }
}
```

## 09 用两个栈实现队列
题目：用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。  
```Java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) {
        stack1.push(node);
    }
    
    public int pop() {
        if (stack2.empty()) {
            if (stack1.empty()) {
                throw new IllegalArgumentException("Queue is empty.");
            }else {
                while (!stack1.empty()) {
                    stack2.push(stack1.pop());
                }
            }
        }
        return stack2.pop();
    }
}
```
## 10 斐波那契数列
**题目**：大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39   
解法：暴力递归。用时较长，因为会有重复计算。

```Java
public class Solution {
    public int Fibonacci(int n) {
        if (n == 0 || n == 1) return n;
        
        return Fibonacci(n-1) + Fibonacci(n-2);
    }
}
```
改进：从前往后遍历。从前往后计算，避免了重复计算。
```Java
public class Solution {
    public int Fibonacci(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("Index is illegal.");
        }
        if (n == 0) return 0;
        if (n == 1) return 1;
        int prePre = 0;
        int pre = 1;
        int result = 0;
        for (int i = 2; i <= n; i++) {
            result = prePre + pre;
            prePre = pre;
            pre = result;
        }
        return result;
    }
}
```
## 10-x 递归的拓展-青蛙跳台阶问题、矩形覆盖问题
青蛙跳台阶：一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级的台阶总共有多少种跳法。  
思路：设跳上 n 级台阶有 f(n) 种跳法。在所有跳法中，青蛙的最后一步只有两种情况： 跳上 1 级或 2 级台阶。
+ 当为 1 级台阶，之前已经跳了 n−1 个台阶，此情况共有 f(n−1) 种跳法；
+ 当为 2 级台阶，之前已经跳了 n−2 个台阶，此情况共有 f(n−2) 种跳法。

故转移方程：*f(n) = f(n-1) + f(n-2)*  
与上一题斐波那契数列不同的是起始数字的不同：

+ 青蛙跳台阶问题：*f(0)=1，f(1)=1，f(2)=2*；  
+ 斐波那契数列：*f(0)=0，f(1)=1，f(2)=2*；  

注意：以上初始条件适用于LeetCode平台。解释：0级台阶青蛙可以选择不跳，即有一种选择。或者从递推公式 *f(2) = f(1) + f(0)* 推出f(0)的值。  

```Java
public class Solution {
    public int JumpFloor(int target) {
        if (target <= 0) {
            return 0;
        }
        if (target <= 2) {
            return target;
        }
        int prePre = 1;
        int pre = 2;
        int result = 0;
        for (int i = 3; i <= target; i++) {
            result = prePre + pre;
            prePre = pre;
            pre = result;
        }
        return result;
    }
}
```
变态跳台阶：一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。  
思路：当n=1时，f(1)=1。当n大于1时，归纳总结可知：跳上n级台阶，第一次跳1级的话，有f(n-1)种方法；第一次跳2级的话，有f(n-2)种方法……第一次跳n-1级的话，有f(1)种方法；直接跳n级的话，有1种方法，所以可以得到如下公式：

*f(n) = f(n-1)+f(n-2)+......f(1)+1　　（n≥2）*
*f(n-1) = f(n-2)+f(n-3)+.....f(1)+1　　（n>2）*

由上面两式相减可得：*f(n)-f(n-1)=f(n-1)*，即*f(n) = 2\*f(n-1)  (n>2)* 最终结合f(1)和f(2)，可以推得：*f(n)=2^(n-1)*  
```Java
public class Solution {
    public int JumpFloorII(int target) {
        if (target <= 0) {
            return 0;
        }
        
        return (int) Math.pow(2, target-1);
    }
}
```
矩形覆盖：我们可以用2 * 1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2 * 1的小矩形无重叠地覆盖一个2 * n的大矩形，总共有多少种方法？   
思路：当n = 1时，有一种方法。当n = 2时，有两种方法。当n >= 3时，和斐波那契数列类似。第一步竖着放，有f(n-1)种方法；第一步横着放，有f(n-2)种方法。所以f(n)=f(n-1)+f(n-2)。  
```Java
public class Solution {
    public int RectCover(int target) {
        if (target <= 0) {
            return 0;
        }
        if (target <= 2) {
            return target;
        }
        int prePre = 1;
        int pre = 2;
        int result = 0;
        for (int i = 3; i <= target; i++) {
            result = prePre + pre;
            prePre = pre;
            pre = result;
        }
        return result;
    }
}
```

## 11 旋转数组的最小数字
**题目**：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。  

第一版：从后往前查找，时间复杂度为O(n)，这种思路没有很好地利用输入的旋转数组的特性。
```Java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        if (array.length == 0) return 0;
        int i;
        for (i = array.length-1; i > 0; i--) {
            if (array[i] < array[i-1]) {
                break;
            }
        }
        return array[i];
    }
}
```
第二版：二分查找  
**思路**：旋转之后的数组实际上可以划分成两个有序的子数组，并且前面子数组的元素都大于后面子数组中的元素。注意到实际上最小的元素就是两个子数组的分界线。本题目给出的数组一定程度上是排序的，因此我们试着用**二分查找法**寻找这个最小的元素。   

1. 我们用两个指针left和right分别指向数组的第一个元素和最后一个元素。按照题目的旋转的规则，第一个元素应该是大于最后一个元素的（如果没有重复的元素的话）。但是如果不是旋转，第一个元素肯定小于最后一个元素。   
2. 找到数组的中间元素。
	+ 若中间元素大于第一个元素，则中间元素位于前面的递增子数组，此时最小元素位于中间元素的后面。我们可以让第一个指针left指向中间元素。移动之后，第一个指针仍然位于前面的递增数组中。
	+ 若中间元素小于第一个元素，则中间元素位于后面的递增子数组，此时最小元素位于中间元素的前面。我们可以让第二个指针right指向中间元素。移动之后，第二个指针仍然位于后面的递增数组中。
	+ 经过上面的移动之后就可以缩小寻找的范围。 
3. 按照以上思路，第一个指针left总是指向前面递增数组的元素，第二个指针right总是指向后面递增的数组元素。最终第一个指针将指向前面数组的最后一个元素，第二个指针指向后面数组中的第一个元素。也就是说他们将指向两个相邻的元素，而第二个指针指向的刚好是最小的元素，这就是循环的结束条件。   

**特殊情况**：对于有重复元素的情况，例如：｛1，0，1，1，1｝ 和 ｛1，1， 1，0，1｝ 都可以看成是递增排序数组｛0，1，1，1，1｝的旋转。此时就无法用上述解法来求解，因为在这两个数组中，第一个数字，最后一个数字，中间数字都是1。第一种情况下，中间数字位于后面的子数组，第二种情况，中间数字位于前面的子数组。  

因此当两个指针指向的数字和中间数字相同的时候，我们无法确定中间数字1是属于前面的子数组还是属于后面的子数组，也就无法移动指针来缩小查找的范围。   
```Java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        if (array == null || array.length <= 0) {
        	return 0;
        }
        int left = 0;
        int right = array.length - 1;
        int mid = 0;
        while (array[left] >= array[right]) {// 注意这里的判断条件，因为是寻找最小元素，如果这个条件不满足，就直接退出了
        	// 终止条件，最终left和right是相邻的，此时：
        	// left将指向前面数组的最后一个元素
        	// right指向后面数组中的第一个元素
            if (right - left == 1) {
                mid = right;// 退出之前将mid指向right的位置，当然这里也可以直接返回right处的值
                break;
            }
            mid = left + (right-left)/2;
            // 特殊情况：如果left、mid、right处的三个元素相等，只能顺序查找
            if (array[left] == array[mid] && array[mid] == array[right]) {
                return minInOrder(array, left, right);
            }
            // 缩小范围进行查找
            if (array[mid] >= array[left]) {
                left = mid;
            }
            if (array[mid] <= array[right]) {
                right = mid;
            }
        }
        return array[mid];
    }
    // 函数功能：顺序查找最小值
    private int minInOrder(int[] array, int left, int right) {
        int result = array[left];
        for (int i = left+1; i <= right; i++) {
            if (array[i] < result) {
                result = array[i];
            }
        }
        return result;
    }
}
```

## 12 矩阵中的路径--回溯法
**题目**：请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用下划线标出）。但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。
 A B T G
 C F C S
 J D E H
**思路**：首先对所整个矩阵遍历，找到第一个字符，然后向上下左右查找下一个字符，由于每个字符都是相同的判断方法（先判断当前字符是否相等，再向四周查找），因此采用递归函数。由于字符查找过后不能重复进入，所以还要定义一个与字符矩阵大小相同的布尔值矩阵，进入过的格子标记为true。如果不满足的情况下，需要进行回溯，此时，要将当前位置的布尔值标记回false。（所谓的回溯无非就是对使用过的字符进行标记和处理后的去标记）

```Java
public class Solution {
    public boolean hasPath(char[] matrix, int rows, int cols, char[] str){
        if (matrix == null || rows < 1 || cols < 1 || str == null) {
            return false;
        }
        //该数组用于记录matrix中的元素是否被访问过，访问过的元素就不能再访问了
        boolean[] isVisited = new boolean[rows * cols];
        for (boolean v : isVisited) {
            v = false;
        }
        int pathLength = 0;//路径长度
        // 两层for循环是为了找到第一个字符
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                if (hasPathCore(matrix, rows, cols, row, col, str, pathLength, isVisited)) {
                    return true;
                }
            }
        }
        return false;
    }
    
    private boolean hasPathCore(char[] matrix, int rows, int cols, int row, int col, 
                                char[] str, int pathLength, boolean[] isVisited) {
        //边界条件不满足，字符已经访问过，字符不匹配，都返回false
        if (row < 0 || row >= rows || col < 0 || col >= cols || 
            isVisited[row*cols+col]==true || matrix[row*cols+col] != str[pathLength]) {
            return false;
        }
        if (pathLength == str.length-1) 
            return true;
        boolean hasPath = false;
        isVisited[row*cols+col] = true;
        hasPath = hasPathCore(matrix, rows, cols, row+1, col, str, pathLength+1, isVisited) ||
            hasPathCore(matrix, rows, cols, row-1, col, str, pathLength+1, isVisited) ||
            hasPathCore(matrix, rows, cols, row, col+1, str, pathLength+1, isVisited) ||
            hasPathCore(matrix, rows, cols, row, col-1, str, pathLength+1, isVisited);
        if (! hasPath) 
            isVisited[row*cols+col] = false;
        
        return hasPath;
    }
}
```
## 13 机器人的运动范围--回溯法
**题目**：地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？  
**自己的收获**：

1. 自己写的代码结构上是对的，在isVisited[row\*cols+col] = true;return 1 + ...这个地方没考虑好，在这个地方就说明（row, col）这个点是符合要求的，要做两件事情：把它标记为已访问，避免重复计数；总的计数值加一。
2. 对于计算数位之和写成一个单独的方法，使得递归函数中的条件判断代码更简洁。  
```Java
public class Solution {
    public int movingCount(int threshold, int rows, int cols){
        if (threshold < 0 || rows < 1 || cols < 1) {
            return 0;
        }
        boolean[] isVisited = new boolean[rows*cols];
        int count = movingCountCore(0,0,threshold,rows,cols,isVisited);
        return count;
    }
    private int movingCountCore(int row,int col,int threshold,
                                int rows,int cols,boolean[] isVisited) {
        //边界条件，是否被访问过，数位是否满足要求
        if (row < 0 || col < 0 || row >= rows || col >= cols ||
           isVisited[row*cols+col] || cal(row)+cal(col) > threshold) {
            return 0;
        }
        //若以上都满足，看此位置的相邻节点
        isVisited[row*cols+col] = true;
        return 1 + movingCountCore(row+1,col,threshold,rows,cols,isVisited)+
            movingCountCore(row-1,col,threshold,rows,cols,isVisited)+
            movingCountCore(row,col+1,threshold,rows,cols,isVisited)+
            movingCountCore(row,col-1,threshold,rows,cols,isVisited);
    }
    private int cal(int num) {
        int sum = 0;
        while (num != 0) {
            sum += num%10;
            num /= 10;
        }
        return sum;
    }
}
```
## 14 剪绳子
**题目**：给你一根长度为n的绳子，请把绳子剪成整数长的m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。  
**总结**：动态规划法，四个特点如下：  

1. 求一个问题的最优解；
2. 整体问题的最优解依赖各子问题的最优解；
3. 小问题之间还有相互重叠的更小的子问题；
4. 为了避免小问题的重复求解，采用**从上往下分析和从下往上求解**的方法求解问题。

```Java
public class Solution {
    public int cutRope(int target) {
        if (target <= 1) return 0;
        if (target == 2) return 1;
        if (target == 3) return 2;
        int[] product = new int[target+1];
        //对于以下四个长度，其本身的数字比乘积大
        //比如长度为4的绳子，分成1和3，如果将1和3再分下去的话，所得结果比本身小
        //故1和3就不再分下去了
        product[0] = 0;
        product[1] = 1;
        product[2] = 2;
        product[3] = 3;
        //i表示绳子的长度
        for (int i = 4; i <= target; i++ ) {
            int max = 0;
            //将该段绳子分成两部分：j和i-j，j的取值如下循环条件
            //看如何分法会得到最大值
            for (int j = 1; j <= i/2; j++) {
                if (max < product[j]*product[i-j]) {
                    max = product[j]*product[i-j];
                }
            }
            product[i] = max;
        }
        return product[target];
    }
}
```
LeetCode中对这道题目的变种，即将绳子的长度放宽，涉及到大数问题。此时使用DP就不太合适，因此使用贪心法来求解。  
首先分析将绳子剪成多少比较合适：1不行，因为 1 * (n-1) < n；2可以，3可以；4拆分成2 * 2与自身大小相等；5不行，因为2 * 3 > 5；大于5的绳子也必须拆，因为总有一种拆法比不拆更优。  
最后的结果只包含2和3(当然当总长度为2和3时单独处理), 那么很显然n >= 5时， 3 * (n - 3) >= 2 *  (n - 2) ，因此我们优先拆成3，最后剩余的拆成2。最后的结果一定是由若干个3和1或2个2组成。（这就是贪心法的思路）  
关于数学推导，参考大佬的解题：[Krahets的题解](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/)  

```java
class Solution {
    public int cuttingRope(int n) {
        // 特殊情况
        if (n == 2) return 1;
        if (n == 3) return 2;

        int mod = (int)1e9+7;//1e9+7是double类型
        long res = 1;
        while (n > 4) {
            res *= 3;
            res %= mod;
            n -= 3;
        }
        return (int)(res * n % mod);
    }
}
```



## 15 二进制中1的个数
**题目**：输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
**思路**：如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。

**举个例子**：一个二进制数```1100```，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是```1011```。我们发现减1的结果是把最右边的一个1开始的所有位都取反了。这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。如```1100&1011=1000```也就是说，**把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0**。那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。  

思路链接：[牛客网-菩提旭光](https://www.nowcoder.com/questionTerminal/8ee967e43c2c4ec193b040ea7fbb10b8?f=discussion)  

```Java
public class Solution {
    public int NumberOf1(int n) {
        int count = 0;
        while (n != 0) {
            n = (n-1) & n;
            count ++;
        }
        return count;
    }
}
```
## 16 数值的整数次方
题目：给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。保证base和exponent不同时为0。  
思路：注意以下陷阱：1）0的负数次方不存在；2）0的0次方没有数学意义；3）要考虑exponent为负数的情况。所以可以对exponent进行分类讨论，在对base是否为0进行讨论。  
收获：
+ 使用**右移运算符>>代替除以2**，有较高的效率：exponent >> 1；
+ 使用**位与运算符代替求余运算符%判断奇偶数**，有较高的效率：if ((exponent & 0x1) == 1)；
+ **做乘方的诀窍**：涉及到求解某数的n次方问题时，可以采用递归来完成，如下公式：
![乘方计算公式](.\images\乘方计算公式.png)  
<center>乘方计算公式</center>
```Java
public class Solution {
    boolean isValid = true; //全局变量来标记是否出错
    public double Power(double base, int exponent) {
        isValid = true;
        double result;
        if (exponent > 0) {
            result = powerCore(base, exponent);
        }else if(exponent < 0) {
            if (base == 0) {
                isValid = false; //0的负数次方不存在
                return 0.0;
            }
            result = 1 / powerCore(base, -exponent);
        }else {
            return 1.0;//此时0的0次方也返回零
        }
        return result;
    }
    
    private double powerCore(double base, int exponent) {
        if (exponent == 0) return 1;
        if (exponent == 1) return base;
        double result = powerCore(base, exponent>>1);
        result *= result;
        //判断exponent是否为奇数的情况，
        //若为奇数，则上述两行少乘了一次base，原因：二进制移位
        if ((exponent & 0x1) == 1) { //这里里面的小括号不要忘记，否则有运算优先级
            result *= base;
        }
        return result;
    }
}
```
非递归实现乘方：
```Java
public double powerCore(double base, int exponent) {
    double result = 0.0;
    //首先判断exponent是奇数还是偶数
    if ((exponent & 0x1) == 1) {
        result = base;
    }
    //指数缩减一半，base平方
    while (exponent > 1) {
        exponent = exponent >> 1;
        base *= base;
    }
    result *= base;
    return result;
}
```
## 17 打印从1到最大的n位数
题目：输入数字n，按顺序打印出从1到最大的n位十进制数。比如输入3，则打印出1、2、3一直到最大的3位数即999。  
思路：n过大时是大数问题，不能简单用int或者long数据输出，需要采用字符数组char[]表达大数。  
```Java
public class Solution {
    public void print1toNNumber(int n) {
        //边界判断
        if (n <= 0)
            return;
        //创建字符数组并初始化，且右边为高位
        char[] digit = new char[n];
        for (int i = 0; i < n; i++) {
            digit[i] = '0';
        }
        for (int i = n-1; i >= 0; i--) {
        	//个位不断加一，直到最高位达到9
            while (digit[i] != '9') {
                int m = 0;
                digit[m]++;//个位加一
                //个位加一之后，判断是否有进位
                while (m < n-1 && digit[m] > '9') {
                    digit[m] = '0';
                    digit[m+1]++;
                    m++;
                }
                printDigit(digit);
            }
        }

    }
    private void printDigit(char[] digit) {
        StringBuilder res = new StringBuilder();
        int m = digit.length-1;
        while (digit[m] == '0') {
            m--;
        }
        for (int i = m; i >= 0; i--) {
            res.append(digit[i]);
        }
        System.out.print(res.toString() + " ");
    }
}
```
LeetCode上的这一道题目：输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。注意：**在函数返回值上有所差别。**  
```java
class Solution {
    public int[] printNumbers(int n) {
        int len = fastPow(10, n)-1;
        int[] res = new int[len];
        for (int i = 0; i < len; i++) {
            res[i] = i+1;
        }
        return res;
    }

    private int fastPow(int base, int index) {
        int result = 1;
        while (index > 0) {
            if ((index&1) == 1) {// 位运算来判断奇偶数
                result *= base;
            }
            index = index>>1;// 移位运算代替除以2的操作
            base *= base;
        }
        return result;
    }
}
```




## 18 删除链表的节点
**在O(1)时间内删除链表节点-题目**：给定单向链表的头指针和一个结点指针，定义一个函数在O(1)时间删除该结点。  
**思路**：通常那样从头开始查找删除需要的时间为O(n)，要在O(1)时间删除某结点，可以这样实现：**设待删除结点i的下一个结点为j，把j的值复制到i，再把i的指针指向j的下一个结点，最后删除j，效果就相当于删除j**。  **注意特殊情况**：1.当待删除结点i为尾结点时，无下一个结点，则只能从头到尾顺序遍历；2.当链表中只有一个结点时（即是头结点，又是尾结点），必须把头结点也设置为null。本题有个**缺陷**：要求O(1)时间删除，相当于隐藏了一个假设：待删除的结点的确在表中。  

```Java
	public ListNode deleteNode(ListNode head, ListNode delNode) {
        if (head == null || delNode == null) return head;
        //如果delNode不是末尾节点，则可以复制下一个节点的内容到delNode，然后删除下一个节点，
        if (delNode.next != null) {
            delNode.val = delNode.next.val;
            delNode.next = delNode.next.next;
        }else if (head == delNode) { //此时只有一个头结点，要删除头结点
            return null;
        }else {
            ListNode curr = head;
            //加上curr != null意思是delNode不一定存在
            while (curr.next != delNode && curr != null) {
                curr = curr.next;
            }
            if(curr == null) {
                System.out.println("无法找到待删除节点！");
                return head;
            }
            curr.next = null;
        }
        return head;
    }
```

**删除链表中重复的节点-题目**：在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5。  
**收获**：不要把重复结点一个一个删除，先定义一个布尔变量确定当前结点是否重复，直到找到不重复的节点，把prev指针指向该不重复的节点，即可批量删除重复节点。
```Java
public class Solution {
    public ListNode deleteDuplication(ListNode pHead){
        if (pHead == null || pHead.next == null) return pHead;
        ListNode prev = null;
        ListNode curr = pHead;
        while (curr != null) {
            boolean needDel = false;
            //如果和后面一个节点值相等，则当前节点需要删除
            if (curr.next != null && curr.val == curr.next.val) {
                needDel = true;
            }
            //如果当前节点不用删除
            if (!needDel) {
                prev = curr;
                curr = curr.next;
            }else { //当前节点是重复的需要删除
                int delValue = curr.val;
                ListNode temp = curr.next; //temp最终指向不重复的节点
                while (temp != null && temp.val == delValue) {
                    temp = temp.next;
                }
                //判断是否开头元素是重复的
                if (prev == null){//说明开头有重复元素被删除了
                    pHead = temp;
                }else {
                    prev.next = temp;
                }
                curr = temp;//更新curr的位置
            }
        }
        return pHead;
    }
}
```
## 19 正则表达式匹配
题目：请实现一个函数用来匹配包括'.'和'\*'的正则表达式。模式中的字符'.'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab\*ac\*a"匹配，但是与"aa.a"和"ab\*a"均不匹配。  
思路：使用**递归**来实现每一步的比较。看**模式字符串**中当前位置的下一个字符是否是“\*”。
1. 模式中当前位置下一个字符是“\*”时：若当前字符不相等，则模式后移两个字符，继续比较；若当前字符相等，则有三种情况：（比如a 和 a\*进行匹配，三种情况使用“||”进行并列比较）
	+ 字符串字符位置不变，模式后移两个字符，继续比较；//表示a\*被忽略
	+ 字符串后移一个字符，模式后移两个字符，继续比较；//表示a 和 a\*匹配
	+ 字符串后移一个字符，模式字符位置不变，继续比较。//表示a 和 a\*匹配，但是星号表示不止一个a。
2. 当模式中当前位置下一个字符不为“\*”时：若当前字符相等，则字符串和模式都后移一个字符，继续调用函数进行比较；若不相等，则返回false。

总结：这道题做的很不好  
1.涉及到数组的情况下，一定要时刻注意数组越界问题！  
2.对于每一步都是采用**相同判断方法**的题目，可以采用递归函数来实现。  
3.思维一定要全面，把握住关键矛盾，将每种情况考虑清楚。例如这道题，关键就在于第二个字符是否为“\*”，确定关键问题后，分析清楚每一种情况即可。  

```Java
public class Solution {
    public boolean match(char[] str, char[] pattern){
        // 边界条件
        if (str == null || pattern == null) {
            return false;
        }
        return matchCore(str, 0, pattern, 0);
    }
    
    private boolean matchCore(char[] str, int s, char[] pattern, int p) {
        // 字符串和模式都递归到最后一个字符的右边了，表示完全匹配
        if (s == str.length && p == pattern.length) {
            return true;
        }
        // 模式不可以先结束，模式先结束肯定是不匹配的，但是字符串如果先结束则不一定，
        // 比如：a和aa*，字符串中的指针先走到length的位置，但此时并没有结束
        // 考虑问题一定要全面
        if (s < str.length && p == pattern.length) {
            return false;
        }
        // 程序的主体是对模式判断下一个字符是否为“*”，模式中的指针最多到倒数第二个。
        if (p+1 < pattern.length && pattern[p+1] == '*') {
            if (s < str.length && 
                (pattern[p] == '.' || pattern[p]==str[s])) {
                // 下面三种情况的含义一定要明确。
                return matchCore(str,s,pattern,p+2)
                    || matchCore(str,s+1,pattern,p+2)
                    || matchCore(str,s+1,pattern,p);
            }else {
            	// 表示当前字符不匹配，模式后移两位，字符串不动，因为字符串中的当前位置还没匹配成功。
                return matchCore(str,s,pattern,p+2);
            }
        }
        // 模式的下一个字符不是“*”，则直接比较字符串和模式中当前位置的字符
        if (s < str.length && (pattern[p] == '.' || pattern[p] == str[s])) {
            return matchCore(str,s+1,pattern,p+1);
        }
        return false;
    }
}
```

动态规划解法：  
```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        boolean[][] dp = new boolean[m+1][n+1];

        // 对原字符串和模式串进行遍历匹配
        // 注意：i和j的意思指的是长度，i=1表示在原字符串第一个字符的位置
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                // 判断模式是否为空
                if (j == 0) {
                    dp[i][j] = i == 0;// 参考dp的边界条件
                }else {
                    // 判断模式中当前字符是否为 * 
                    if (p.charAt(j-1) != '*') {
                        if (i > 0 && (s.charAt(i-1)==p.charAt(j-1) 
                        || p.charAt(j-1)=='.')) {
                            dp[i][j] = dp[i-1][j-1];
                        }
                    }else {
                        // 直接忽略
                        if (j >= 2) {
                            dp[i][j] |= dp[i][j-2];
                        }
                        // 星号前面的字符匹配上了
                        if (i>=1 && j>=2 && (s.charAt(i-1)==p.charAt(j-2) 
                        || p.charAt(j-2)=='.')) {
                            dp[i][j] |= dp[i-1][j];
                        }
                    }
                }
            }
        }
        return dp[m][n];
    }
}
```

## 20 表示数值的字符串
题目：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。  
思路：将数字的形式总结为：```A.B E/e C``` ，按顺序进行判断，其中A和C代表有符号整数，B代表无符号整数。上述是完整形式，有几个特殊状况需要考虑：有A的话，没有B也是可以的，即```3.```和```2.e-1```等；没有A的话必须有B，即```.3```是可以的。  

总结：
1、没想到```A.B E/e C```这样的数字格式；  
2、对于全局变量的理解不够到位，所以写出了这样的函数：```private boolean isInteger(char[] str, int index);```
3、注意几个地方：

```Java
isNumeric = isUnsignedInteger(str) || isNumeric; //isNumeric的顺序
if (isNumeric && index == str.length){} //最后对index是否到达结尾进行判断
```
4、LeetCode中这一题的输入是String类型，字符串的首尾可能存在空格，所以首先要去掉空格：```s = s.trim()```


```Java
public class Solution {
    private int index = 0;//记录当前位置
    public boolean isNumeric(char[] str) {
        if (str == null || str.length == 0) 
            return false;
        boolean isNumeric = isInteger(str);//判断A
        //判断B
        if (index < str.length && str[index] == '.') {
            index++;
            isNumeric = isUnsignedInteger(str) || isNumeric;//A. .B A.B都可以
        }
        //判断e后面的A
        if (index < str.length && (str[index] == 'e' || str[index] == 'E')) {
            index++;
            isNumeric = isInteger(str) && isNumeric;
        }
        if (isNumeric && index == str.length) {
            return true;
        }else {
            return false;
        }
    }
    private boolean isInteger(char[] str) {
        if (index < str.length && (str[index] == '+' || str[index] == '-')) {
            index++;
        }
        return isUnsignedInteger(str);
    }
    
    private boolean isUnsignedInteger(char[] str) {
        int start = index;
        while (index < str.length && str[index] >= '0' && str[index] <= '9') {
            index++;
        }
        if (index > start) {
            return true;
        }
        return false;
    }
}
```
## 21 调整数组顺序使奇数位于偶数前面
题目：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。  
方法一：插入排序。  
```Java
public class Solution {
    public void reOrderArray(int[] array) {
        if (array == null || array.length == 0) {
            return;
        }
        for (int i = 0; i < array.length; i++) {
            if (!isOdd(array[i])) {
                continue;
            }
            int temp = array[i];
            int j = i;
            for (; j > 0; j--) {
                if (!isOdd(array[j-1])) {
                    array[j] = array[j-1];
                }else {
                    break;
                }
            }
            array[j] = temp;
        }
    }
    
    private boolean isOdd(int num) {
        return (num & 0x1) == 1;
    }
}
```
方法二：归并排序。  
上述方法在LeetCode中用时570ms，巨慢。在上述方法的基础上改进了一版，改为归并排序，用时10ms。（注：LeetCode中函数返回值是int[]）  
```java
class Solution {
    public int[] exchange(int[] nums) {
        if (nums == null || nums.length == 0) {
            return new int[]{};// 返回一个空数组，而不是null
        }
        // 归并排序，对奇偶性，而不是大小
        merge(nums, 0, nums.length-1);
        return nums;
    }

    private void merge(int[] nums, int left, int right) {
        // 递归到底的情况：只剩一个元素。
        if (left >= right) {
            return;
        }

        int mid = (left+right)>>1;
        merge(nums, left, mid);
        merge(nums, mid+1, right);
        // 对两部分进行奇偶性排序之后，再将两部分并起来
        mergeCore(nums, left, mid, right);
        return ;
    }
    private void mergeCore(int[] nums, int left, int mid, int right) {
        int[] numsCopy = Arrays.copyOfRange(nums, left, right+1);
        int i = left;
        int j = mid+1;// i j是原数组中的索引，和拷贝数组的偏移为left
        for (int k = left; k <= right; k++) {
            if (i > mid) {// 左边处理完了
                nums[k] = numsCopy[j-left];
                j++;
            }else if (j > right) {// 右边处理完了
                nums[k] = nums[i-left];
                i++;
            }else {
                // 此时是两边都没处理完
                if ((numsCopy[i-left]&1) == 1) {
                    nums[k] = numsCopy[i-left];
                    i++;
                }else {
                    nums[k] = numsCopy[j-left];
                    j++;
                }
            }
        }
        return;
    }
}
```
方法三：双指针。  
上述10ms的用时只击败了5.32%的用户，继续改进。参考了题解采用双指针的方法。考虑定义双指针 i , j 分列数组左右两端，循环执行：
+ 指针 i 从左向右寻找偶数；
+ 指针 j 从右向左寻找奇数；
+ 将 偶数 nums[i] 和 奇数 nums[j] 交换。

```java
class Solution {
    public int[] exchange(int[] nums) {
        if (nums == null || nums.length == 0) {
            return new int[]{};// 返回一个空数组，而不是null
        }
        // 双指针的方法
        int i = 0; 
        int j = nums.length-1;
        while (i < j) {
            while (i < j && ((nums[i]&1) == 1)) {// 注意运算符优先级：关系运算符>位运算符>逻辑运算符
                i++;
            }
            while (i < j && ((nums[j]&1) == 0)) {
                j--;
            }
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
        return nums;
    }
}
```
这道题目的变体：小Q最近遇到了一个难题：把一个字符串的大写字母放到字符串的后面，各个字符的相对位置不变，且不能申请额外的空间。 你能帮帮小Q吗？  题目来源：[腾讯笔试题](https://blog.csdn.net/weixin_44233929/article/details/105686614)  
思路：这道题目要保证各个字符相对位置不变，但是不能使用额外的空间，因此用插入排序的思想。时间复杂度是O(n<sup>2</sup>)，不能使用额外的空间，时间复杂度自然就高了。归并排序也可以保证相对位置不变，但是要使用额外的空间。

## 22 链表中倒数第k个节点
**题目**：输入一个链表，输出该链表中倒数第k个结点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾结点是倒数第1个结点。例如一个链表有6个结点，从头结点开始它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个结点是值为4的结点。  
第一版：两次遍历。  
```Java
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        if (head == null) return null;
        ListNode curr = head;
        while (curr != null) {
            k--;
            curr = curr.next;
        }
        //此时k大于链表长度
        if (k > 0) return null;
        //此时倒数第k个是头结点
        if (k == 0) return head;
        //根据k的值找到目标节点的前一个节点
        curr = head;
        while (++k != 0) {
            curr = curr.next;
        }
        return curr.next;
    }
}
```
第二版：利用两个相距为k-1的指针。设置两个指针，第一个指针先遍历k-1步；从第k步开始，第二个指针指向头结点，两个结点同时往后遍历，当第一个指针到达最后一个结点时，第二个指针指向的正好是倒数第k个结点。  
> 牛客网评论：“相当于制造了一个K长度的尺子，把尺子从头往后移动，当尺子的右端与链表的末尾对齐的时候，尺子左端所在的结点就是倒数第k个结点”。

```Java
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        if (head == null || k <= 0) return null;
        ListNode pAhead = head;
        for (int i = 1; i < k; i++) {
            pAhead = pAhead.next;
            //判断k是否大于链表长度，方法：考虑边界情况，即链表长度也为k
            if (pAhead == null) return null;
        }
        ListNode pBehind = head;
        //当pAhead在最后一个节点时，pBehind即为倒数第k个节点
        while (pAhead.next != null) {
            pAhead = pAhead.next;
            pBehind = pBehind.next;
        }
        return pBehind;
    }
}
```

## 23 链表中环的入口节点
题目：给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。  
方法一：使用集合来存储节点。  
```Java
import java.util.Set;
import java.util.HashSet;
public class Solution {
    public ListNode EntryNodeOfLoop(ListNode pHead){
        if (pHead == null) return null;
        Set<ListNode> set = new HashSet<>();
        while (pHead != null) {
            set.add(pHead);
            if (set.contains(pHead.next)) {
                return pHead.next;
            }
            pHead = pHead.next;
        }
        return null;
    }
}
```
方法二：快慢指针。  
思路：分为三步：
1. 首先确定链表中是否有环：通过两个不同速度的指针确定，当两个指针指向同一个结点时，该结点为环中的一个结点。
2. 确定环中结点的数目n：指针走一圈，边走边计数
3. 找到环的入口：从头结点开始，通过两个相差为n的指针来得到（即寻找链表中倒数第n个结点）。

```Java
public class Solution {
    public ListNode EntryNodeOfLoop(ListNode pHead){
        ListNode meetingNode = meeting(pHead);
        if (meetingNode == null) return null;
        //此时链表中有环形，meetingNode为环形链表中的某个节点
        //下一步算出环形链表的长度count
        int count = 1;
        ListNode node1 = meetingNode.next;
        while (node1 != meetingNode) {
            count++;
            node1 = node1.next;
        }
        //下一步根据环形的长度来确定环形入口
        //nodeLeft和nodeRight是相距为count的两个节点
        ListNode nodeRight = pHead;
        ListNode nodeLeft = pHead;
        for (int i = 1; i <= count; i++) {
            nodeRight = nodeRight.next;
        }
        
        while (nodeLeft != nodeRight) {
            nodeLeft = nodeLeft.next;
            nodeRight = nodeRight.next;
        }
        return nodeLeft;
        
    }
    //确定链表中是否存在环形，若存在则返回环形中的一个节点，不存在则返回null
    private ListNode meeting(ListNode pHead) {
        if (pHead == null) return null;
        ListNode pSlow = pHead;
        ListNode pFast = pHead;
        while (pFast != null) {
            pSlow = pSlow.next;
            pFast = pFast.next;
            if (pFast != null) 
                pFast = pFast.next;
            if (pSlow == pFast) 
                return pFast;
        }
        return null;
    }
}
```
## 24 反转链表
**题目**：输入一个链表，反转链表后，输出新链表的表头。  
方法一：三个指针。  
```Java
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode prev = null, next = null;
        ListNode curr = head;
        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```
方法二：递归。  
```Java
public class Solution {
    public ListNode ReverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode newHead = ReverseList(head.next);
        head.next.next = head;//这句代码是将head接在它邻居的后面
        head.next = null;
        return newHead;
    }
}
```
## 25 合并两个排序的链表
**题目**：输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。  
非递归版本：  
```Java
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        if (list1 == null) return list2;
        if (list2 == null) return list1;
        ListNode newHead = list1.val < list2.val ? list1 : list2;
        ListNode curr = newHead;
        list1 = newHead == list1 ? list1.next : list1;
        list2 = newHead == list2 ? list2.next : list2;
        while (list1!=null && list2!=null) {
            if (list1.val < list2.val) {
                curr.next = list1;
                curr = list1;
                list1 = list1.next;
            }else {
                curr.next = list2;
                curr = list2;
                list2 = list2.next;
            }
        }
        if (list1 == null) {
            curr.next = list2;
        }else {
            curr.next = list1;
        }
        return newHead;
    }
}
```
递归版本：  
```Java
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        if (list1 == null) return list2;
        if (list2 == null) return list1;
        if (list1.val < list2.val) {
            list1.next = Merge(list1.next, list2);
            return list1;
        }else {
            list2.next = Merge(list1, list2.next);
            return list2;
        }
    }
}
```
## 26 树的子结构
题目：输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）  
思路：
1. 先对A树进行遍历，找到与B树的根结点值相同的结点R；
2. 判断A树中以R为根结点的子树是否包含B树一样的结构。

总结：在函数twoTreeOrder里面对于递归终止条件没把握好。对子结构没理解好，一开始以A作为主体，拿B去和A比较，实际上应该以B为主体。如下图示：
![](.\images\二叉树.jpg)  
```Java
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if (root1 == null || root2 == null) 
            return false;
        return  preOrder(root1, root2);
    }
    
    private boolean preOrder(TreeNode root1, TreeNode root2) {
        //递归终止条件
        if (root1 == null) {
            return false;
        }
        //找到了头结点，然后比较子树
        if (root1.val == root2.val) {
            if (twoTreeOrder(root1,root2))
                return true;
        }
        return preOrder(root1.left, root2) || preOrder(root1.right, root2);
    }
    
    private boolean twoTreeOrder(TreeNode root1, TreeNode root2) {
        
        if (root2 == null) {
            return true;//递归终止条件，即root1=root2=null
        }else {
            if (root1 == null) 
                return false;
            if (root1.val != root2.val)
                return false;
        }
        return twoTreeOrder(root1.left,root2.left) 
            && twoTreeOrder(root1.right,root2.right);
    }
}
```

## 27 二叉树的镜像
**题目**：请完成一个函数，输入一个二叉树，该函数输出它的镜像。  
```Java
public class Solution {
    public void Mirror(TreeNode root) {
        if (root == null) return ;
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        Mirror(root.left);
        Mirror(root.right);
        return ;
    }
}
```
## 28 对称的二叉树
题目：请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。  
```Java
public class Solution {
    public boolean isSymmetrical(TreeNode root){
        if (root == null) return true;
         return isSymm(root.left, root.right);
    }
    
    private boolean isSymm(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) 
                return true;//递归终止条件
        if (root1 == null || root2 == null) 
                return false;
        return root1.val == root2.val
            && isSymm(root1.left, root2.right)
            && isSymm(root1.right, root2.left);
    }
}
```
## 29 顺时针打印矩阵
题目：输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10。  
注意：重复打印的错误，注意单行或者单列时是否会重复打印。  
```Java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printMatrix(int [][] matrix) {
       if (matrix == null || matrix.length == 0) 
           return null;
        ArrayList<Integer> list = new ArrayList<>();
        int nl=0, ml=0;
        int nr = matrix.length-1;
        int mr = matrix[0].length-1;
        while (true) {
            for (int i = ml; i <= mr; i++) 
                list.add(matrix[nl][i]);
            nl++;
            if (nl > nr)
                break;
            
            for (int i = nl; i <= nr; i++) 
                list.add(matrix[i][mr]);
            mr--;
            if (ml > mr)
                break;

            for (int i = mr; i >= ml; i--) 
                list.add(matrix[nr][i]);
            nr--;
            if (nl > nr)
                break;

            for (int i = nr; i >= nl; i--) 
                list.add(matrix[i][ml]);
            ml++;
            if (ml > mr)
                break;
        }
        return list;
   }
}
```
## 30 包含min函数的栈
**题目**： 定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。注意：保证测试中不会当栈为空的时候，对栈调用pop()或者min()或者top()方法。  
**思路**：采用一个辅助栈来存放最小值，压入时，把每次的最小元素（之前最小元素与新入栈元素的较小值）保存起来放到辅助栈中
　　　　栈  3，4，2，5，1 (top)
　    辅助栈  3，3，2，2，1 (top)
总结：对题目的用意没理解好，我原先的理解是利用Java提供的Stack来实现一个自定义的栈，自定义栈能在O(1)时间内弹出最小值，我期望在自定义栈中能实现数据从小到大排列。参考别人的解题后，发现题目是要求出当前栈中最小值是多少，就是说只需要知道当前的最小值，并不要对原数据进行排列。  

```Java
import java.util.Stack;
public class Solution {
    Stack<Integer> stack_data = new Stack<>();
    Stack<Integer> stack_min = new Stack<>();
    public void push(int node) {
        stack_data.push(node);
        if (stack_min.empty() || node < stack_min.peek()) {
            stack_min.push(node);
        } else {
            stack_min.push(stack_min.peek());
        }
    }
    
    public void pop() {
        if (!stack_data.empty()) {
            stack_data.pop();
            stack_min.pop();
        }
    }
    
    public int top() {
        return stack_data.peek();
    }
    
    public int min() {
        return stack_min.peek();
    }
}
```
## 31 栈的压入、弹出序列
题目：输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）  
思路：新建一个栈，依次先入栈，再判断是否出栈；出栈序列能实现栈的清空说明两个序列匹配。  
```Java
import java.util.Stack;
public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        if (pushA == null || popA == null) 
            return false;
        if (pushA.length != popA.length || pushA.length == 0) 
            return false;
        Stack<Integer> stack = new Stack<>();
        int popIndex = 0;
        for (int pushIndex=0; pushIndex<pushA.length; pushIndex++) {
            stack.push(pushA[pushIndex]);
            while (!stack.empty() && popA[popIndex]==stack.peek()) {
                stack.pop();
                popIndex++;
            }
        }
        return stack.empty();
    }
}
```
## 32 从上到下打印二叉树
**题目**：从上往下打印出二叉树的每个节点，同层节点从左至右打印。  

```Java
import java.util.ArrayList;
import java.util.Queue;
import java.util.LinkedList;
public class Solution {
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode delNode = queue.remove();
            res.add(delNode.val);
            if (delNode.left != null) {
                queue.add(delNode.left);
            }
            if (delNode.right != null) {
                queue.add(delNode.right);
            }
        }
        return res;
    }
}
```
**把二叉树打印成多行**  
题目：从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。  
```Java
import java.util.ArrayList;
import java.util.LinkedList;
public class Solution {
    public ArrayList<ArrayList<Integer>> Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if (pRoot == null) {
            return res;
        }
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(pRoot);
        int pCount = 0;//当前层节点个数
        int nextCount = 1;//下一层节点个数，初始值这样设置的原因是循环里面有pCount = nextCount
        while (!queue.isEmpty()) {
            pCount = nextCount;
            nextCount = 0;
            ArrayList<Integer> pLevel = new ArrayList<>();
            for (int i = 0; i < pCount; i++) {
                TreeNode delNode = queue.remove();
                pLevel.add(delNode.val);
                if (delNode.left != null) {
                    queue.add(delNode.left);
                    nextCount++;
                }
                if (delNode.right != null) {
                    queue.add(delNode.right);
                    nextCount++;
                }
            }
            res.add(pLevel);
        }
        return res;
    }
}
```
**按之字形顺序打印二叉树**  
题目：请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。  
思路：采用两个栈，对于不同层的结点，一个栈用于正向存储，一个栈用于逆向存储，打印出来就正好是相反方向。  
```Java
import java.util.Stack;
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if (pRoot == null) {
            return res;
        }
        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        TreeNode delNode = null;
        stack1.push(pRoot);
        while (!stack1.empty() || !stack2.empty()) {
            ArrayList<Integer> pLevel = new ArrayList<>();
            while (!stack1.empty()) {
                delNode = stack1.pop();
                pLevel.add(delNode.val);
                if (delNode.left != null) {
                    stack2.push(delNode.left);
                }
                if (delNode.right != null) {
                    stack2.push(delNode.right);
                }
            }
            res.add(pLevel);
            
            // 二叉树的高度为奇数时，stack1 pop()完之后stack2为空，防止添加一个空的list
            if (stack2.empty()) {
                break;
            }
            
            pLevel = new ArrayList<>();
            while (!stack2.empty()) {
                delNode = stack2.pop();
                pLevel.add(delNode.val);
                if (delNode.right != null) {
                    stack1.push(delNode.right);
                }
                if (delNode.left != null) {
                    stack1.push(delNode.left);
                }
            }
            res.add(pLevel);
        }
        return res;
    }

}
```
## 33 二叉搜索树的后序遍历序列
**题目**：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。 
**思路**：后序遍历序列。当前区间[left, right]内最后一个元素sequence[right]为该树的根节点，在[left, right]中找到第一个大于等于sequence[right]的位置mid，等于sequence[right]的情况是该树没有右子树。然后将[left, right)分为左右两部分进行递归判断。  
```Java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        if (sequence == null) {
        	return false;
        }
        if (sequence.length == 0) {// 空数组是空树的后序遍历序列
        	return true;
        }
        return verifyCore(sequence, 0, sequence.length-1);
    }
    
    private boolean verifyCore(int[] sequence, int left, int right) {
        // 递归终止条件，这里的判断条件可以用 >= 或者 > ，用“>”更好，这是递归到底（空节点）的情况。注意：不能用==，因为left会大于right
        if (left > right) {
        	return true;
        }
        // 通过大小关系找到左右子树的分界点
        // 终止条件是sequence[mid] >= sequence[right]，等号意味着没有右子树，mid和right重合
        int mid = left;
        while (sequence[mid] < sequence[right]) {
            mid++;
        }
        // 判断右子树中的只是否都是大于sequence[right]的
        for (int i = mid; i < right; i++) {
            if (sequence[i] <= sequence[right])
                return false;
        }
        // 递归判断左右子树
        return verifyCore(sequence,left,mid-1) && 
            verifyCore(sequence,mid,right-1);
    }
}
```
## 34 二叉树中和为某一值的路径
**题目**：输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)  
**思路**：
1. 假设找到了其中一条路径，达到叶结点后，由于没有指向父结点的指针，所以必须提前创建一个列表存储前面经过的结点。  
2. 由于是从根结点出发，所以要想到使用前序遍历  
3. 利用列表存储结点，在该结点完成左右子树的路径搜索后（即递归函数结束，返回到其父结点之前），要删除该结点，从而记录别的路径。  
```Java
/*
 * 几个要点：
 * 1. 将nodeList和pathList定义成全局变量，避免在方法中的多次传递
 * 2. 在pathList中添加nodeList时，因为nodeList会不断变化，所以必须新建一个list存入
 *    复制ArrayList的方法：newList=new ArrayList<Integer>(oldList)(复制内容，而不是复制地址，注意与newList=oldList的区分）
 * 3. 在当前结点完成左右子树的路径搜索后，记得删除nodeList中的当前结点
 * 4. target是基本数据类型int，不会受到方法的影响而改变
 */
import java.util.ArrayList;

public class Solution {
    ArrayList<ArrayList<Integer>> pathList = new ArrayList<>();
    ArrayList<Integer> nodeList = new ArrayList<>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if (root == null) 
            return pathList;
        nodeList.add(root.val);
        target -= root.val;
        if (target == 0 && root.left==null && root.right==null) {//叶子节点
            //找到插入pathList中的位置
            int i = 0;
            while (i < pathList.size() 
                   && nodeList.size() < pathList.get(i).size())
                i++;
            pathList.add(i, new ArrayList<Integer>(nodeList));//复制nodeList中的内容，这一步特别注意
        }else {
            pathList = FindPath(root.left, target);
            pathList = FindPath(root.right, target);
        }
        nodeList.remove(nodeList.size()-1);//将当前节点删除
        return pathList;
    }
}
```
## 35 复杂链表的复制
**题目**：输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）  
**思路**：复制原始结点N创建N’，将N'链接到N的后面；根据原始结点N的sibling可以快速设置N'结点的sibling，最后将这个长链表拆分成原始链表和复制链表（根据奇偶位置）  
**总结**：
1. 涉及链表结点操作，**必须时刻注意对null的判断**；
2. 复制链表时，**在原始结点后面直接插入复制结点**，这种方法非常方便，有较高的时间效率，先记住，以后可能会遇到类似的应用。
```Java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
public class Solution {
    public RandomListNode Clone(RandomListNode head){
        if (head == null) 
            return null;
        cloneNode(head);                 //1.复制节点
        setRandomNode(head);             //2.设置random指针
        return reconnectLinkedList(head);//3.拆分长链表
    }
    
    //复制每个结点，并插入到原始节点的后面
    private void cloneNode(RandomListNode head) {
        RandomListNode pNode = head;
        while (pNode != null) {
            RandomListNode cloneNode = new RandomListNode(pNode.label);
            cloneNode.next = pNode.next;
            pNode.next = cloneNode;
            pNode = cloneNode.next;
        }
    }
    
    //根据原结点的random指针，设置复制结点的random指针
    private void setRandomNode(RandomListNode head) {
        RandomListNode pNode = head;
        while (pNode != null) {
            RandomListNode cloneNode = pNode.next;
            if (pNode.random != null)
                cloneNode.random = pNode.random.next;
            else 
                cloneNode.random = null;
            pNode = cloneNode.next;
        }
    }
    
    //将长链表拆分成原始链表和复制链表（根据奇偶位置）
    private RandomListNode reconnectLinkedList(RandomListNode head) {
        RandomListNode pNode = head;
        RandomListNode cloneHead = null;
        RandomListNode cloneNode = null;
        if (head != null) {
            cloneHead = head.next;
            cloneNode = cloneHead;
            pNode.next = cloneNode.next;
            pNode = pNode.next;
        }
        while (pNode != null) {
            cloneNode.next = pNode.next;
            cloneNode = cloneNode.next;
            pNode.next = cloneNode.next;//保证了原链表的结构不变
            pNode = pNode.next;
        }
        return cloneHead;
    }
}
```
## 36 二叉搜索树与双向链表***
**题目**：输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。  
**思路**：**中序遍历**。要实现双向链表，必须知道当前结点的前一个结点lastNode。根据中序遍历可以知道，当遍历到根结点的时候，左子树已经转化成了一个排序的链表了，根结点的前一结点就是该链表的最后一个结点（这个结点必须记录下来，将遍历函数的返回值设置为该结点即可），链接根结点和前一个结点，此时链表最后一个结点就是根结点了。再处理右子树，遍历右子树，**将右子树的最小结点与根结点链接起来即可**。左右子树的转化采用递归即可。  
**收获**：lastNode的巧妙设置；对递归的理解。  
```Java
public class Solution {
    public TreeNode Convert(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode lastNode = null;
        lastNode = convertCore(root, lastNode);
        TreeNode firstNode = lastNode;
        while (firstNode.left != null) {
            firstNode = firstNode.left;
        }
        return firstNode;
    }
    private TreeNode convertCore(TreeNode node, TreeNode lastNode) {
        //处理左子树，获得最大节点
        if (node.left != null) {
            lastNode = convertCore(node.left, lastNode);
        }
        //链接此时的最大节点和根节点node
        node.left = lastNode;
        if (lastNode != null) {
            lastNode.right = node;
        }
        //左子树处理完，最大节点指向了node，因为node的值比左子树都大
        lastNode = node;
        //处理右子树
        if (node.right != null) {
            lastNode = convertCore(node.right, lastNode);
        }
        return lastNode;//返回以node为根的树中的最大节点
    }
}
```
第二次复习写的代码，LeetCode平台上的这一题，题目有所区别：
**题目**：输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的**循环**双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。  
```Java
class Solution {
    public Node treeToDoublyList(Node root) {
        // 边界条件
        if (root == null) {
            return null;
        }
		// 不断向左遍历来确定头结点head
        Node head = root;
        while (head.left != null) {
            head = head.left;
        }

        // 就地中序遍历
        Node tail = inorder(root);
        // 头尾指针的处理，过程循环链表
        head.left = tail;
        tail.right = head;
        return head;
    }

    // 函数功能：以node为根的树，进行中序遍历转化为双向链表，返回的是尾节点
    private Node inorder(Node node) {
        // 左子树为空，则跳过
        if (node.left != null) {
            Node leftNode = inorder(node.left);
            leftNode.right = node;
            node.left = leftNode;
        }

        if (node.right == null) {
            return node;
        }
        // 找到node的前驱节点，即node的右子树中最小的节点
        Node successor = node.right;
        while (successor.left != null) {
            successor = successor.left;
        }
        Node rightNode = inorder(node.right);
        node.right = successor;
        successor.left = node;
        return rightNode;
    }
}
```

## 37 序列化二叉树***
**题目**： 请实现两个函数，分别用来序列化和反序列化二叉树。  
二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（#），以 ！ 表示一个结点值的结束（value!）。  
二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。  
**收获**：
+ 对于str可以将其分割成字符数组，但这里使用了一个全局的位置指针，通过substring()方法来截取字符串，从而达到节省空间的目的。
+ 字符串的比较使用equals()来比较。
+ 数组长度arr.length，字符串的长度str.length()
```Java
public class Solution {
    public String Serialize(TreeNode root) {
        StringBuilder res = new StringBuilder();
        if (root == null) {
            res.append("#!");
        }else {
            res.append(root.val+"!");
            res.append(Serialize(root.left));
            res.append(Serialize(root.right));
        }
        return res.toString();
    }
    
    int position = 0;//使用一个全局的位置指针来取str中的数据
    public TreeNode Deserialize(String str) {
        TreeNode node = null;
        if (str == null || str.length() <= 0) {
            return node;
        }
        int start = position;
        //找到下一个分隔点，循环结束，position停在'!'位置上
        while (str.charAt(position) != '!') {
            position++;
        }
        if (!str.substring(start, position).equals("#")) {
            node = new TreeNode(Integer.parseInt(str.substring(start,position)));
            position++;
            node.left = Deserialize(str);//这里的递归思路要明确，为什么可以传入str！
            node.right = Deserialize(str);
        }else {//'#'表示为null，position向后，此时node=null
            position++;
        }
        return node;
    }
}
```
## 38 字符串的排列
**题目**：输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。  
**思路**：将字符串看成两部分，一部分是第一个字符，另一部分是后面的所有字符。首先确定第一个字符，该字符可以是字符串中的任意一个；固定第一个字符后，求出后面所有字符的排列（**相同步骤，采用递归**）。实现细节：实现第一个字符的改变，只需要将第一个字符和后面所有字符进行交换即可。要记得字符串输出后要将字符交换回来，变回原始的字符串。  

```Java
import java.util.ArrayList;
import java.util.Collections;
public class Solution {
    public ArrayList<String> Permutation(String str) {
        ArrayList<String> list = new ArrayList<>();
        if (str == null) {
            return list;
        }
        char[] strArray = str.toCharArray();
        permutation(strArray,0,list);
        Collections.sort(list);//字符串排序
        return list;
    }
    private void permutation(char[] strArray, int index, ArrayList<String> list) {
        if (index == strArray.length-1) {
            //判断是否有重复字符串，不重复则添加
            if (!list.contains(String.valueOf(strArray))) {
                list.add(String.valueOf(strArray));
            }
        } else {
            for (int i = index; i < strArray.length; i++) {
                char temp = strArray[index];
                strArray[index] = strArray[i];
                strArray[i] = temp;
                permutation(strArray, index+1, list);
                strArray[i] = strArray[index];
                strArray[index] = temp;
            }
        }
    }
}
```
时间复杂度的改进：上述代码是在排列完了之后再看是否有重复，在LeetCode中运行直接超时，在策略上改进了一下，运用剪枝的思想，将字符存在Set里面，如果遇到重复的字符直接跳过。注意LeetCode中返回值不同。  
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Set;
import java.util.HashSet;
import java.util.Collections;
class Solution {
    public String[] permutation(String s) {
        if (s == null || s.length() == 0) {
            return new String[]{};
        }
        List<String> list = new ArrayList<>();
        char[] charArr = s.toCharArray(); 
        permutationCore(charArr, 0, list);
        Collections.sort(list);
        return list.toArray(new String[0]);
    }

    public void permutationCore(char[] charArr, int index, List<String> list) {
        if (index == charArr.length-1) {
            list.add(String.valueOf(charArr));// 这里就不用考虑是否有重复了
            return ;
        }
        Set<Character> set = new HashSet<>();
        for (int i = index; i < charArr.length; i++) {
            if (set.contains(charArr[i])) {
                continue;// 重复则进行剪枝，比如abba，index为0时，for循环到最后两位时就不会进行递归了
            }
            set.add(charArr[i]);// 如果不重复要将字符加到Set里面。
            swap(charArr, index, i);
            permutationCore(charArr, index+1, list);
            swap(charArr, index, i);
        }
    }
    private void swap(char[] charArr, int i, int j) {
        char temp = charArr[i];
        charArr[i] = charArr[j];
        charArr[j] = temp;
    }
}
```


## 39 数组中出现次数超过一半的数字
题目：数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。  
总结：
+ 特殊情况没考虑全，即array中只有一个元素的情况没考虑到。
+ 求数组长度的一半可以用移位运算，又没想到。  

思路一：使用哈希表保存每个数以及对应的次数。  
```Java
import java.util.HashMap;
public class Solution {
    public int MoreThanHalfNum_Solution(int[] array) {
        if (array == null || array.length == 0) {
            return 0;
        }
        HashMap<Integer, Integer> map = new HashMap<>();
        int n = array.length;
        for (int num : array) {
            if (!map.containsKey(num)) {
                if (n == 1) {
                    return num;//array中只有一个元素的情况
                }
                map.put(num,1);
            }else {
                int times = map.get(num)+1;
                if (times > n/2)
                    return num;
                map.put(num, times);
            }
        }
        return 0;
    }
}
```
思路二：数字次数超过一半，则说明：该数字出现的次数比其他数字之和还多。  
具体实现：遍历数组过程中保存两个值：一个是数组中某一数字，另一个是次数。遍历到下一个数字时，若与保存数字相同，则次数加1，反之减1。若次数=0，则保存下一个数字，次数重新设置为1。由于要找的数字出现的次数比其他数字之和还多，那么要找的数字肯定是最后一次把次数设置为1的数字。  

```Java
public class Solution {
    public int MoreThanHalfNum_Solution(int[] array) {
        if (array == null || array.length == 0) {
            return 0;
        }
        int num = array[0];
        int count = 1;
        for (int i = 1; i < array.length; i++) {
            if (count == 0) {
                num = array[i];
                count = 1;
            } else if (array[i] == num) {
                count++;
            } else {
                count--;
            }
        }
        if (count > 0) {
            int times = 0;
            for (int i = 0; i < array.length; i++) {
                if (num == array[i])
                    times++;
            }
            if (times > array.length>>1) {
                return num;
            }
        }
        return 0;
    }
}
```
## 40 最小的k个数
题目：输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。  
第一版：选择排序。  

```Java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int[] input, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        if (k <= 0 || k > input.length) return res;
        for (int i = 0; i < k; i++) {
            int min = i;
            for (int j = i+1; j < input.length; j++) {
                if (input[j] < input[min]) {
                    min = j;
                }
            }
            swap(input, i, min);
            res.add(input[i]);
        }
        return res;
    }
    
    private void swap(int[] input, int index1, int index2) {
        int temp = input[index1];
        input[index1] = input[index2];
        input[index2] = temp;
    }
}
```
第二版：堆排序。  
```Java
import java.util.ArrayList;
import java.util.PriorityQueue;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int[] input, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        if (input == null || k <= 0 || k > input.length) {
            return res;
        }
        PriorityQueue<Integer> heap = new PriorityQueue<>(k,(e1,e2)->(e2-e1));
        for (int i = 0; i < input.length; i++) {
            if (heap.size() < k) {
                heap.offer(input[i]);
            }else if (input[i] < heap.peek()) {
                heap.poll();
                heap.offer(input[i]);
            }
        }
        for (int i : heap) {
            res.add(i);
        }
        return res;
    }
}
```

## 41 数据流中的中位数
**题目**：如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。  
**思路**：  
所谓数据流，就是不会一次性读入所有数据，只能一个一个读取，每一步都要求能计算中位数。  

将读入的数据分为两部分，一部分数字小，另一部分大。小的一部分采用大顶堆存放，大的一部分采用小顶堆存放。当总个数为偶数时，使两个堆的数目相同，则中位数=大顶堆的最大数字与小顶堆的最小数字的平均值；而总个数为奇数时，使小顶堆的个数比大顶堆多一，则中位数=小顶堆的最小数字。  

因此，插入的步骤如下：
1. 若已读取的个数为偶数（包括0）时，两个堆的数目已经相同，将新读取的数插入到小顶堆中，从而实现小顶堆的个数多一。但是，如果新读取的数字比大顶堆中最大的数字还小，就不能直接插入到小顶堆中了 ，此时必须将新数字插入到大顶堆中，而将大顶堆中的最大数字插入到小顶堆中，从而实现小顶堆的个数多一。  
2. 若已读取的个数为奇数时，小顶堆的个数多一，所以要将新读取数字插入到大顶堆中，此时方法与上面类似。  

**总结**：

1. PriorityQueue的常用方法有：offer(Object)、poll()、size()、peek()等。
2. 这道题关键在于分成两个平均分配的部分，奇偶时分别插入到最大最小堆中，利用最大最小堆性质的插入方法要掌握。  
```Java
import java.util.PriorityQueue;
import java.util.Comparator;
public class Solution {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(11, new Comparator<Integer>(){
        @Override
        public int compare(Integer a, Integer b){
            return b-a;
        }
    });
    public void Insert(Integer num) {
        if ( ((minHeap.size()+maxHeap.size())&0x1) == 0 ) {//当前个数为偶数，下一个数字加入小顶堆
            if (!maxHeap.isEmpty() && maxHeap.peek() > num) {
                maxHeap.offer(num);
                num = maxHeap.poll();
            }
            minHeap.offer(num);
        }else {//目前的个数为奇数，下一个数字加入大顶堆
            if (!minHeap.isEmpty() && minHeap.peek() < num) {
                minHeap.offer(num);
                num = minHeap.poll();
            }
            maxHeap.offer(num);
        }
    }

    public Double GetMedian() {
        if (minHeap.size()+maxHeap.size() == 0) {
            return null;
        }
        double median;
        if ( ((minHeap.size()+maxHeap.size())&0x1) == 0) {
            median =( minHeap.peek()+maxHeap.peek() ) / 2.0;
        }else {
            median = minHeap.peek();
        }
        return median;
    }
}
```
LeetCode上的这一题，自己在刷题时，使用大顶堆和小顶堆，采用了另外一种代码思路，即先比较加入的数字和两个堆顶元素的大小关系，加入堆中，然后再平衡两个堆中元素的个数。代码如下：  
```java
import java.util.PriorityQueue;
class MedianFinder {

    /** initialize your data structure here. */
    private PriorityQueue<Integer> heapSmall;//大顶堆，存放排序之后前一半的数字
    private PriorityQueue<Integer> heapBig;//小顶堆，存放排序之后后一半的数字
                                            // 排序是将元素加到优先队列中自然进行的
    public MedianFinder() {
        heapSmall = new PriorityQueue<>((a,b) -> (b-a));
        heapBig = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        if (!heapSmall.isEmpty() && num < heapSmall.peek()) {
            heapSmall.offer(num);
            if (heapSmall.size() > heapBig.size()) {
                heapBig.offer(heapSmall.poll());
            }
        }else if (!heapBig.isEmpty() && num > heapBig.peek()) {
            heapBig.offer(num);
            if (heapBig.size() - heapSmall.size() > 1) {
                heapSmall.offer(heapBig.poll());
            }
        }else {
            // 都为空的情况
            heapBig.offer(num);
            if (heapBig.size() - heapSmall.size() > 1) {
                heapSmall.offer(heapBig.poll());
            }
        }
    }
    
    public double findMedian() {
        if (heapBig.size() > heapSmall.size()) {
            return (double)heapBig.peek().intValue();
        }else {
            return (heapSmall.peek()+heapBig.peek())/2.0;
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```


## 42 连续子数组的最大和
**题目**：输入一个整型数组，数组里有正数也有负数。数组中一个或连续的多个整/数组成一个子数组。求所有子数组的和的最大值。要求时间复杂度为O(n)。  
**思路**：分析规律，从第一个数字开始累加，若走到某一个数字时，前面的累加和为负数，说明不能继续累加了，要从当前数字重新开始累加。在累加过程中，将每次累加和的最大值记录下来，遍历完成后，返回该数字。  

```Java
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        if (array == null || array.length <= 0) return 0;
        int maxSum = array[0];
        int sum = array[0];
        for (int i = 1; i < array.length; i++) {
            if (sum < 0) {
                sum = 0;
            }
            sum += array[i];
            if (sum > maxSum) {
                maxSum = sum;
            }
        }
        return maxSum;
    }
}
```
## 43 1~n整数中1出现的次数
题目：输入一个整数n，求从1到n这n个整数的十进制表示中1出现的次数。例如输入12，从1到12这些整数中包含1 的数字有1，10，11和12，1一共出现了5次。  
思路：  
对于整数n，我们将这个整数分为三部分：当前位数字cur，更高位数字high，更低位数字low，如：对于n=21034，当位数是十位时，cur=3，high=210，low=4。我们从个位到最高位 依次计算每个位置出现1的次数：  

1. 当前位的数字等于0时，例如n=21034，在百位上的数字cur=0，百位上是1的情况有：00100~00199，01100~01199，……，20100~20199。一共有21\*100种情况，即high\*100;  
2. 当前位的数字等于1时，例如n=21034，在千位上的数字cur=1，千位上是1的情况有：01000~01999，11000~11999，21000~21034。一共有2\*1000+（34+1）种情况，即high\*1000+(low+1)。  
3. 当前位的数字大于1时，例如n=21034，在十位上的数字cur=3，十位上是1的情况有：00010~00019，……，21010~21019。一共有（210+1）\*10种情况，即(high+1)\*10。  

这个方法只需要遍历每个位数，对于整数n，其位数一共有lgn个，所以时间复杂度为O(logn)。  
```Java
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        int count = 0;
        for (int i=1; i<=n; i*=10) {//i代表当前位的权重
            int high = n/(i*10);//当前位之前的高位数字
            int low = n%i;//当前位之后的低位数字
            int curr = (n/i)%10;//当前位的数字
            if (curr == 0) {
                count += high*i;
            }else if (curr == 1) {
                count += high*i + low + 1;
            }else {
                count += (high+1)*i;
            }
        }
        return count;
    }
}
```
## 44 数字序列中某一位的数字
题目：数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从0开始计数）是5，第13位是1，第19位是4，等等。请写一个函数求任意位对应的数字。  
思路：观察出如下规律
+ 个位数的个数一共有10个，即0~9，共占了10\*1位数字；
+ 两位数的个数一共有90个，即10~99，每个数字占两位，共占了90\*2位数字；
+ ……
+ m位数的个数一共有9\*10^(m-1)个，每个数字占m位，占了9\*10^(m-1)\*m位数字。

判断第index对应的目标数字属于几位数，然后找到目标数字，再从目标数字中进行寻找。  
```Java
public class PositionInNumberSequence {
    public int digitAtIndex(int index) {
        if (index < 0) {
            return -1;
        }
        int m = 1;//记录位数
        while (true) {
            int numbers = getNumbersOfm(m);//m位的整数个数
            if (index < numbers*m) {
                return getDigit(index, m);
            }
            index -= numbers*m;//减去numbers对应的字符数
            m++;
        }
    }

    //返回m位数的总个数，例如，两位数一共有90个：10~99；三位数有900个：100~999
    private int getNumbersOfm(int m) {
        if (m == 1) {
            return 10;
        }
        return (int) (9*Math.pow(10,m-1));
    }

    //获取目标数字中的字符
    private int getDigit(int index, int m) {
        int firstNumber = getFirstNumber(m);
        int number = firstNumber + index / m;//目标数字
        int indexFromRight = m - index % m;
        for (int i = 1; i < indexFromRight; i++) {
            number /= 10;
        }
        return number%10;//取出个位上的字符
    }

    private int getFirstNumber(int m) {
        if (m == 1) {
            return 0;
        }
        return (int) Math.pow(10,m-1);
    }

    public static void main(String[] args) {
        PositionInNumberSequence demo = new PositionInNumberSequence();
        System.out.println(demo.digitAtIndex(0)==0);
        System.out.println(demo.digitAtIndex(1)==1);
        System.out.println(demo.digitAtIndex(19)==4);
        System.out.println(demo.digitAtIndex(1000)==3);
        System.out.println(demo.digitAtIndex(1001)==7);
        System.out.println(demo.digitAtIndex(1002)==0);
    }
}
```
## 45 把数组排成最小的数
**题目**：输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。  
**思路**：对于数字m和n，可以拼接成mn和nm，**如果mn<nm，我们定义m小于n**。反之则相反。利用这个排序规则，从小排到大即可实现题目要求。拼接m和n时，要考虑到**大数问题**，因此将m和n拼接起来的数字转换成字符串处理。因为mn和nm的字符串位数相同，因此它们的大小只需要按照字符串大小的比较规则就可以了。  
**具体实现**：将数字存入ArrayList中，通过利用Collections.sort(List\<T> list, Comparator\<? super T> c)方法进行排序。Comparator中重写compare()方法来规定比较规则。  
**个人总结**：compareTo()和compare()方法搞混了。compareTo(Object o)--Comparable\<T>接口--可以理解为**内部排序**，compare(Object o1, Object o2)是Comparator\<T>接口的方法--比较器可以理解为**外部排序**。参考资料：[java：compareTo和compare方法之比较](https://blog.csdn.net/fly910905/article/details/81670353)  

```Java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
public class Solution {
    public String PrintMinNumber(int[] numbers) {
        if (numbers == null || numbers.length <= 0) 
            return "";
        ArrayList<String> list = new ArrayList<>();
        for (int num : numbers) {
            list.add(String.valueOf(num));
        }
        Collections.sort(list, new Comparator<String>(){
            public int compare(String s1, String s2) {
                String str1 = s1+s2;
                String str2 = s2+s1;
                return str1.compareTo(str2);
            }
        });
        StringBuilder res = new StringBuilder();
        for (String str : list) {
            res.append(str);
        }
        return res.toString();
    }
}
```
## 46 把数字翻译成字符串
题目：给定一个数字，我们按照如下规则把它翻译为字符串：0翻译成"a"，1翻译成"b"，……，11翻译成"l"，……，25翻译成"z"。一个数字可能有多个翻译。例如12258有5种不同的翻译，它们分别"bccfi", "bwfi", "bczi", "mcfi" 和"mzi" 。请编程实现一个函数用来计算一个数字有多少种不同的翻译方法。  
思路：**从个位往前递归**。注意：边界条件以及num%100 <= 25 && num%100 >= 10，因为101中01不能翻译为b，只能翻译为ab。  

```Java
public class NumToStr {
    private int total = 0;
    public int numToStr(int num) {
        if (num < 0) {
            return 0;
        }

        //递归终止条件
        if (num/10 == 0) {
            return 1;
        }

        if (num%100 <= 25 && num%100 >= 10) {
            return total + numToStr(num/10) + numToStr(num/100);
        }else {
            return total + numToStr(num/10);
        }
    }

    public static void main(String[] args) {
        NumToStr demo = new NumToStr();
        System.out.println(demo.numToStr(0));
        System.out.println(demo.numToStr(10));
        System.out.println(demo.numToStr(101));
        System.out.println(demo.numToStr(5858));
        System.out.println(demo.numToStr(12258));
        System.out.println(demo.numToStr(-100));
    }
}
```
## 47 礼物的最大价值--动态规划
题目：在一个m×n的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格直到到达棋盘的右下角。给定一个棋盘及其上面的礼物，请计算你最多能拿到多少价值的礼物？  
思路：动态规划。  
代码思路一：递归实现。

```Java
public class MaxValueOfGifts {
    public int maxValueOfGifts(int[][] value) {
        //边界判断
        if(value==null || value.length==0 || value[0].length==0) {
            return 0;
        }
        return maxValue(value, 0, 0);
    }
    private int maxValue(int[][] value, int n, int m) {
        if (n == value.length-1 && m == value[0].length-1) {
            return value[n][m];
        }
        if (n == value.length-1) {
            return value[n][m] + maxValue(value,n,m+1);
        }else if (m == value[0].length-1) {
            return value[n][m] + maxValue(value,n+1, m);
        }else {
            return value[n][m] + Math.max(maxValue(value,n+1, m), maxValue(value,n,m+1));
        }
    }
    public static void main(String[] args) {
        int[][] value = new int[][]{{1, 10, 3, 8},{12,2,9,6},
                                    {5, 7, 4, 11},{3, 7, 16, 5}};
        MaxValueOfGifts demo = new MaxValueOfGifts();
        System.out.println(demo.maxValueOfGifts(value));//53
    }
}
```
代码思路二：循环实现，节省了很多递归计算中的重复计算。
```Java
public int maxValueOfGifts2(int[][] value) {
    if (value == null || value.length == 0 || value[0].length == 0) {
        return 0;
    }
    int rows = value.length;
    int cols = value[0].length;
    int[][] maxValue = new int[rows][cols];
    for (int i = rows-1; i >= 0; i--) {
            for (int j = cols-1; j >= 0; j--) {
                int low = 0;
                int right = 0;
                if (i < rows-1) {
                    low = maxValue[i+1][j];
                }
                if (j < cols-1) {
                    right = maxValue[i][j+1];
                }
                maxValue[i][j] = Math.max(low, right) + value[i][j];
            }
        }
        return maxValue[0][0];
}
```
对空间的优化：辅助数组不用和m * n的二维数组一样大，只需要保存上一层的最大值就可以。可以使用一维数组来保存计算结果。  

## 48 最长不含重复字符的子字符串--动态规划
题目：请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。假设字符串中只包含从'a'到'z'的字符。  
第一版：使用集合来存储字符，遇到重复元素就清空集合，遍历结束集合长度的最大值就为最长子字符串的长度。  

```Java
import java.util.TreeSet;
public class LongestSubstringWithoutDup {
    public int getMaxLength(String str) {
        if (str == null || str.length() == 0) {
            return 0;
        }
        TreeSet<Character> set = new TreeSet<>();
        int maxLen = 0;
        for (int i = 0; i < str.length(); i++) {
            if (set.contains(str.charAt(i))) {
                if (maxLen < set.size()) {
                    maxLen = set.size();
                }
                set.clear();
                set.add(str.charAt(i));
            }else {
                set.add(str.charAt(i));
            }
        }
        if (maxLen < set.size()) {
            maxLen = set.size();
        }
        return maxLen;
    }
}
```
第二版：动态规划。定义函数f(i)：以第i个字符为结尾的不含重复字符的子字符串的最大长度。
当第i个字符之前未出现过，则有：f(i)=f(i-1)+1；  
当第i个字符之前出现过，记该字符与上次出现的位置距离为d：  

+ 如果d<=f(i-1)，则有f(i)=d；
+ 如果d>f(i-1)，则有f(i)=f(i-1)+1。

我们从第一个字符开始遍历，定义两个int变量preLength和curLength来分别代表f(i-1)和f(i)，再创建一个长度为26的pos数组来存放26个字母上次出现的位置，即可根据上述说明进行求解。  

```Java
public class LongestSubstringWithoutDup {
    public int getMaxLength2(String str) {
        if (str == null || str.length() == 0) {
            return 0;
        }
        int preLength = 0;//即f(i-1)
        int curLength = 0;//即f(i)
        int maxLength = 0;
        int[] pos = new int[26];//用于存放字母上次出现的位置
        for (int i = 0; i < pos.length; i++) {
            pos[i] = -1;
        }
        for (int i = 0; i < str.length(); i++) {
            int letterNumber = str.charAt(i) - 'a';
            if (pos[letterNumber] < 0 || i - pos[letterNumber] > preLength) {
                curLength = preLength+1;
            }else {
                curLength = i - pos[letterNumber];
            }
            pos[letterNumber] = i;
            if (curLength > maxLength) {
                maxLength = curLength;
            }
            preLength = curLength;
        }
        return maxLength;
    }

    public static void main(String[] args) {
        LongestSubstringWithoutDup demo = new LongestSubstringWithoutDup();
        System.out.println(demo.getMaxLength2("arabcacfr")==4);
        System.out.println(demo.getMaxLength2("a")==1);
        System.out.println(demo.getMaxLength2("aaa")==1);
        System.out.println(demo.getMaxLength2("abcdef")==6);
        System.out.println(demo.getMaxLength2("")==0);
        System.out.println(demo.getMaxLength2(null)==0);
    }
}
```
## 49 丑数
题目：把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。  
思路：  
第一版：暴力法。  
```Java
public class Solution {
    public int GetUglyNumber_Solution(int index) {
        if (index <= 0) return 0;
        if (index == 1) return 1;
        int num = 1;
        while (index > 1) {
            num++;
            //判断当前的num是不是丑数
            if (isUgly(num)) {
                index--;
            }
        }
        return num;
    }
    private boolean isUgly(int num) {
        while (num > 1) {
            if ((num & 0x1) == 0) {
                num /= 2;
            }else if (num%3 == 0) {
                num /= 3;
            }else if (num%5 == 0) {
                num /= 5;
            }else {
                return false;
            }
        }
        return true;
    }
}
```
第二版：三个队列。  
思路：丑数的因子只有2,3,5，那么丑数p = 2 ^ x * 3 ^ y * 5 ^ z，所以一个丑数一定由另一个丑数乘以2或者乘以3或者乘以5得到。维护三个队列，队列中的元素是丑数数组乘以2,3,5得到，将三个队列中对首最小的元素取出放入丑数数组中，依此思路从而达到index个。三个队列在代码实现中设计成三个指针即可。具体分析过程见：[牛客网--事无巨细，悉究本末](https://www.nowcoder.com/questionTerminal/6aa9e04fc3794f68acf8778237ba065b?f=discussion)  

```Java
public class Solution {
    public int GetUglyNumber_Solution(int index) {
        //0~6的丑数为自身
        if (index < 7) return index;
        //p2，p3，p5分别为三个队列的指针，newNum为从队列头选出来的最小数
        int p2 = 0, p3 = 0, p5 = 0, newNum = 1;
        int[] arr = new int[index];
        int p = 0;
        arr[p] = newNum;
        while (p+1 < index) {
            //选出三个队列头最小的数
            newNum = Math.min(arr[p2]*2, Math.min(arr[p3]*3,arr[p5]*5));
            //这三个if有可能进入一个或者多个，进入多个是三个队列头最小的数有多个的情况
            if (arr[p2]*2 == newNum) p2++;
            if (arr[p3]*3 == newNum) p3++;
            if (arr[p5]*5 == newNum) p5++;
            arr[++p] = newNum;
        }
        return newNum;
    }
}
```

## 50 第一个只出现一次的字符
**题目**：在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.   
**思路**：创建哈希表，键值key为字符，值value为出现次数。第一遍扫描：对每个扫描到的字符的次数加一；第二遍扫描：对每个扫描到的字符通过哈希表查询次数，第一个次数为1的字符即为符合要求的输出。  
由于字符（char）是长度为8的数据类型，共有256种可能，因此哈希表可以用一个长度为256的数组来代替，数组的下标相当于键值key，对应字符的ASCII码值；数组的值相当于哈希表的值value，用于存放对应字符出现的次数。
**总结**：如果需要创建哈希表，键为字符，值为数字时，可以考虑用数组（length=256）来替代，数组下标表示为字符的ASCII码值。  

```Java
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        if (str == null) {
            return -1;
        }
        int[] repetitions = new int[256];
        for (int i = 0; i < repetitions.length; i++) {
            repetitions[i] = 0;
        }
        for (int i = 0; i < str.length(); i++) {
            int loc = (int) str.charAt(i);
            repetitions[loc] += 1;
        }
        for (int i = 0; i < str.length(); i++) {
            int loc = (int) str.charAt(i);
            if (repetitions[loc] == 1) {
                return i;
            }
        }
        return -1;
    }
}
```
LeetCode中的这一题：在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。
思路：哈希表和有序哈希表。
哈希表中的Value用Boolean来表示，因为只要多余一次就是重复的字符，相比于整型递增使用Boolean更加节省空间，另一方面也简化了代码逻辑。对于题目中第一个的理解：两次for循环遍历都是按照字符串s的顺序进行遍历的，所以第二次遍历得到的肯定是第一个只出现一次的字符，否则就没找到。
```java
class Solution {
    public char firstUniqChar(String s) {
        // 边界条件
        if (s == null || s.length() == 0) {
            return ' ';
        }
        char[] chars = s.toCharArray();
        HashMap<Character, Boolean> map = new HashMap<>();//Map中的Value使用Boolean节省空间，简化代码逻辑
        for (char ch : chars) {
            // true: 没有重复， false: 有重复
            map.put(ch, !map.containsKey(ch));
        }
        for (char ch : chars) {
            if (map.get(ch)) {
                return ch;
            }
        }
        return ' ';
    }
}
```
有序哈希表：在哈希表的基础上，有序哈希表中的键值对是 按照插入顺序排序 的。哈希表是 去重 的，即哈希表中键值对数量 ≤ 字符串 s 的长度。因此，相比于方法一，方法二减少了第二轮遍历的循环次数。当字符串很长（重复字符很多）时，方法二则效率更高。
```java
class Solution {
    public char firstUniqChar(String s) {
        // 边界条件
        if (s == null || s.length() == 0) {
            return ' ';
        }
        char[] chars = s.toCharArray();
        Map<Character, Boolean> map = new LinkedHashMap<>();//Map中的Value使用Boolean节省空间，简化代码逻辑
        for (char ch : chars) {
            // true: 没有重复， false: 有重复
            map.put(ch, !map.containsKey(ch));
        }

        for (Character ch : map.keySet()) {
            if (map.get(ch)) {
                return ch;
            }
        }
        return ' ';
    }
}
```

## 50-2 字符流中第一个只出现一次的字符
题目：请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是'g'。当从该字符流中读出前六个字符"google"时，第一个只出现一次的字符是'l'。  
第一种思路：用StringBuilder将字符保存起来，以及用一个数组模拟哈希表来存储各个字符出现的次数。（自己写的）
```Java
public class Solution {
    private int[] charMap = new int[256];
    private StringBuilder chars = new StringBuilder();
    //Insert one char from stringstream
    public void Insert(char ch){
        chars.append(ch);
        int loc = (int)ch;
        charMap[loc] += 1;
    }
    //return the first appearence once char in current stringstream
    public char FirstAppearingOnce(){
        for (int i = 0; i < chars.length(); i++) {
            char ch = chars.charAt(i);
            if (charMap[(int)ch] == 1) {
                return ch;
            }
        }
        return '#';
    }
}
```
第二种写法：不用将各个字符保存起来，只需保存索引即可，而且索引可以放在数组中，节省了空间。  
```Java
public class Solution {
    private int index;// 字符流中各个字符的索引
    private int[] charMap;
    public Solution() {
        index = 0;
        charMap = new int[256];
        for (int i = 0; i < 256; i++) {
            charMap[i] = -1;
        }
    }
    //Insert one char from stringstream
    public void Insert(char ch) {
        int loc = (int)ch;
        if (charMap[loc] == -1) {// 表示未出现过
            charMap[loc] = index;
        }else {//表示已经出现过一次，或者出现两次或两次以上
            charMap[loc] = -2;
        }
        index++;
    }
    //return the first appearence once char in current stringstream
    public char FirstAppearingOnce() {
        int minIndex = Integer.MAX_VALUE;
        char resChar = '#';
        for (int i = 0; i < 256; i++) {
        	// 取出≥0的索引中，最小的索引对应的字符
            if (charMap[i] >= 0 && charMap[i] < minIndex) {
                minIndex = charMap[i];
                resChar = (char)i;
            }
        }
        return resChar;
    }
}
```
总结：流和串的区别
+ 串：字符串已经保存下来了，能够读取遍历，因此在字符串中第一个只出现一次的字符中，只需要存下每个字符出现的个数，然后直接在字符串中遍历；  
+ 流：字符流没有存下来，无法进行遍历，因此在本题中，只能在数据容器哈希表中遍历，而且哈希表中存放的是对应字符的位置，而不是个数。  


## 51 数组中的逆序对
题目：在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007  
思路：归并排序的思想，先将数组分解成为n个长度为1的子数组，然后进行两两合并同时排好顺序。  

在对两个子区域合并排序时，记左边区域（下标为start~mid）的指针为i，右边区域（下标为mid+1~end）的指针为j，两个指针都指向该区域内最大的数字，排序时：
1. 如果i指向的数字大于j指向的数字，说明：逆序对有j-mid个，我们把i指向的数字放入临时创建的排序数组中，然后令i-1，指向该区域前一个数字，继续进行排序；
2. 如果i指向的数字小于等于j指向的数字，说明暂时不存在逆序对，将j指向的数字放入临时创建的排序数组中，然后令j-1，指向该区域前一个数字，继续进行排序；
3. 某一子区域数字都放入排序数组后，将另一个子区域剩下的数字放入排序数组中，完成排序；
4. 最后将排序好的数字按顺序赋值给原始数组的两个子区域，以便合并后的区域与别的区域合并。

```Java
public class Solution {
    public int InversePairs(int [] array) {
        // 边界判断
        if (array == null || array.length <= 0) {
            return 0;
        }
        return getCount(array,0,array.length-1);
    }
    // 返回数组array中区间[start, end]之间的逆序对的个数
    private int getCount(int[] array, int start, int end) {
        // 递归终止条件
        if (start >= end) {
            return 0;
        }
        int mid = (start+end)>>1;
        int leftCount = getCount(array, start, mid)%1000000007;
        int rightCount = getCount(array, mid+1, end)%1000000007;
        
        // 以下是归并的过程 //
        int count = 0;
        int i = mid;
        int j = end;
        int[] temp = new int[end-start+1];// 归并时临时保存元素的数组
        int k = end-start;// 临时数组中的指针，从末尾开始，因为大数先进来
        while (i >= start && j >= mid+1) {
            if (array[i] > array[j]) {
                count += (j-mid);
                if (count >= 1000000007) {
                    count %= 1000000007;
                }
                temp[k--] = array[i--];
            }else {
                temp[k--] = array[j--];
            }
        }
        // 把剩下的元素复制到临时数组中
        while (i >= start) {
            temp[k--] = array[i--];
        }
        while (j >= mid+1) {
            temp[k--] = array[j--];
        }
        
        // 把临时数组中的元素复制到原数组中，使得这一段是排序的
        for (k = 0; k < temp.length; k++) {
            array[k+start] = temp[k];
        }
        
        return (count+leftCount+rightCount)%1000000007;
    }
}
```
## 52 两个链表的第一个公共节点
题目：输入两个链表，找出它们的第一个公共结点。  
方法一：利用长度关系：计算两个链表的长度之差，长链表先走相差的步数，之后长短链表同时遍历，找到的第一个相同的结点就是第一个公共结点。  
```Java
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        if (pHead1 == null || pHead2 == null) {
            return null;
        }
        // 首先确定两个链表的长度差
        int len1 = getLength(pHead1);
        int len2 = getLength(pHead2);
        int lenDiff = len1 - len2;
        ListNode longList = pHead1;
        ListNode shortList = pHead2;
        if (lenDiff<0) {
            lenDiff = -lenDiff;
            longList = pHead2;
            shortList = pHead1;
        }
        
        // 根据长度差，长链表先走lenDiff步
        for (int i = 0; i < lenDiff; i++) {
            longList = longList.next;
        }
        while ( (longList != null) && (longList != shortList)) {
            longList = longList.next;
            shortList = shortList.next;
        }
        return longList;
    }
    // 返回链表长度
    private int getLength(ListNode head) {
        int len = 0;
        ListNode cur = head;
        while (cur != null) {
            len++;
            cur = cur.next;
        }
        return len;
    }
}
```
方法二：利用两个指针：一个指针顺序遍历list1和list2，另一个指针顺序遍历list2和list1，（这样两指针能够保证最终同时走到尾结点），两个指针找到的第一个相同结点就是第一个公共结点。（通俗一点理解就是，1+3 == 3+1，故p1和p2最终一定会相遇）
```Java
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode p1 = pHead1;
        ListNode p2 = pHead2;
        while (p1 != p2) {
            p1 = p1==null ? pHead2 : p1.next;
            p2 = p2==null ? pHead1 : p2.next;
        }
        return p1;
    }
}
```
## 53-1 数字在排序数组中出现的次数
题目：统计一个数字在排序数组中出现的次数。  
思路：对于例子来说，如果采用二分法找到某一个3后，再往前遍历和往后遍历到第一个和最后一个3，在长度为n的数组中有可能出现O(n)个3，因此这样的扫描方法时间复杂度为O(n)，效率与从头到尾扫描一样，速度太慢。

这题关键是找到第一个和最后一个3，因此我们尝试**改进二分法**：中间数字比3大或者小的情况与之前类似，关键是中间数字等于3的情况，这时可以分类讨论如下：
1. 如果中间数字的前一个数字也等于3，说明第一个3在前面，继续在前半段查找第一个3；
2. 如果中间数字的前一个数字不等于3，说明该位置是第一个3；
3. 如果中间数字的后一个数字也等于3，说明最后一个3在后面，继续在后半段查找最后一个3；
4. 如果中间数字的后一个数字不等于3，说明该位置是最后一个3；

```Java
public class Solution {
    public int GetNumberOfK(int[] array , int k) {
        // 边界条件
        if (array == null || array.length <= 0) {
            return 0;
        }
        
        int firstK = getFirstK(array,0,array.length-1,k);
        if (firstK == -1) {
            return 0;
        }
        int lastK = getLastK(array,firstK,array.length-1,k);
        return lastK-firstK+1;
    }
    // 返回k第一次出现的位置
    private int getFirstK(int[] array, int left, int right, int k) {
        if (left > right) {
            return -1;
        }
        int mid = (left+right)>>1;
        if (array[mid] == k) {
            if (mid == 0 || array[mid-1] != k) {
                return mid;
            }else {
                right = mid-1;
            }
        }else if (array[mid] > k) {
            right = mid-1;
        }else {
            left = mid+1;
        }
        return getFirstK(array,left,right,k);
    }
    
    // 返回k最后一次出现的位置
    private int getLastK(int[] array, int left, int right, int k) {
        while (left <= right) {
            int mid = (left+right)>>1;
            if (array[mid] == k) {
                if (mid == array.length-1 || array[mid+1] != k) {
                    return mid;
                }else {
                    left = mid+1;
                }
            }else if(array[mid] > k) {
                right = mid-1;
            }else {
                left = mid+1;
            }
        }
        return -1;
    }
}
```
## 53-2 0到n-1中缺失的数字
题目：一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0到n-1之内。在范围0到n-1的n个数字中有且只有一个数字不在该数组中，请找出这个数字。  
思路：二分查找。当中间数字等于其下标时，我们在后半部分查找；当中间数字不等于其下标时（只可能是小于），在前部分查找。代码中中间位置选取的是靠右的中间位置（对于left+right是奇数的情况）。  
```java
public class MissingNumber {
    public int findMissingNumber(int[] array) {
        // 输入不合法的情况
        if (array == null || array.length == 0) {
            return -1;
        }

        // 找缺失数字
        int left = 0;
        int right = array.length-1;
        int mid = 0;
        while (left <= right) {
            mid = (left+right+1)>>1;
            if (array[mid] == mid) {
                left = mid+1;
            }else {
                right = mid-1;
            }
        }
        return left;
    }
}
```
## 53-3 数组中数值和下标相等的元素
题目：假设一个单调递增的数组里的每个元素都是整数并且是唯一的。请编程实现一个函数找出数组中任意一个数值等于其下标的元素。例如，在数组{-3, -1,1, 3, 5}中，数字3和它的下标相等。  
思路：二分查找。  
```Java
public class IntegerIdenticalToIndex {
    public int getNumberSameAsIndex(int[] array) {
        // 边界条件
        if (array == null || array.length <= 0) {
            return -1;
        }
        int left = 0;
        int right = array.length-1;
        while (left <= right) {
            int mid = (left+right)>>1;
            if (mid == array[mid]) {
                return mid;
            } else if (mid > array[mid]) {
                left = mid+1;
            }else {
                right = mid-1;
            }
        }
        return -1;
    }
}
```
## 54 二叉搜索树的第k个节点
**题目**：给定一棵二叉搜索树，请找出其中的第k小的结点。例如：5，3，7，2，4，6，8（层序遍历）中，按结点数值大小顺序第三小结点的值为4。  
**思路**：设置全局变量index=0，对BST进行中序遍历，每遍历一个结点，index+1，当index=k时，该结点即为所求结点。  
```Java
public class Solution {
    private int index = 0;
    public TreeNode KthNode(TreeNode root, int k){
        TreeNode pNode = null;
        if (root == null || k < 1) {
            return pNode;
        }
        pNode = getKthNode(root, k);
        return pNode;
    }
    
    private TreeNode getKthNode(TreeNode node, int k) {
        TreeNode curNode = null;
        if (node.left != null) {
            curNode = getKthNode(node.left, k);
        }
        
        if (curNode == null) {
            index++;
            if (index == k) {
                curNode = node;
            }
        }
        
        if (curNode == null && node.right != null) {
            curNode = getKthNode(node.right, k);
        }
        return curNode;
    }
}
```
LeetCode上的这一题：二叉搜索树的第k大节点。
区别：第k大节点，上面是第k小，原理都是一样的（中序遍历）。这里的返回值是int。注意全局变量的使用，我一开始用的是List来存储从大到小的数字，完全没必要。  
```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private int count;
    private int result = Integer.MAX_VALUE;
    public int kthLargest(TreeNode root, int k) {
        // 边界条件
        if (root == null || k < 1) {
            return result;
        }
        count = k;
        inorder(root);
        return result;
    }

    private void inorder(TreeNode node) {
        if (node == null) {
            return ;
        }
        
        inorder(node.right);
        if (count == 1) {
            result = node.val;
            count--;// 这里需要把count--因为下面的return并没有退出整个递归函数
            return ;
        }
        count--;
        inorder(node.left);
        return ;
    }
}
```


## 55 二叉树的深度
题目：输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。  
```Java
public class Solution {
    public int TreeDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int depth = 1;
        return treeDepth(root, depth);
    }
    
    private int treeDepth(TreeNode node, int depth) {
        if (node == null) {
            return depth;
        }

        int leftDepth = node.left==null ? depth : treeDepth(node.left, depth+1);
        int rightDepth = node.right==null ? depth : treeDepth(node.right, depth+1);
        
        return Math.max(leftDepth, rightDepth);
    }
}
```
简洁版代码：
```Java
public class Solution {
    public int TreeDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftDep = TreeDepth(root.left);
        int rightDep = TreeDepth(root.right);
        return 1+Math.max(leftDep, rightDep);
    }
}
```
## 55-2 平衡二叉树
**题目**：输入一棵二叉树的根结点，判断该树是不是平衡二叉树。如果某二叉树中任意结点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。  
**思路**：计算树的深度，树的深度=max(左子树深度，右子树深度)+1。在遍历过程中，判断左右子树深度相差是否超过1，如果不平衡，则令树的深度=-1，用来表示树不平衡。最终根据树的深度是否等于-1来确定是否为平衡树。  

```Java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        return getDepth(root) != -1;
    }
    
    private int getDepth(TreeNode node) {
        if (node == null) return 0;
        int left = getDepth(node.left);
        if (left == -1) return -1;// 如果发现有任一子树不平衡，立马根据递归顺序返回，不再遍历剩余节点
        int right = getDepth(node.right);
        if (right == -1) return -1;
        return Math.abs(left-right) > 1 ? -1 : 1+Math.max(left,right);
    }
}
```
第二次写的代码：
```Java
class Solution {
    public boolean isBalanced(TreeNode root) {
        // 递归终止条件
        if (root == null) {
            return true;
        }

        return isBalanced(root.left) && isBalanced(root.right)
        && Math.abs(getDepth(root.left)-getDepth(root.right))<=1;
    }

    private int getDepth(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return 1+Math.max(getDepth(node.left), getDepth(node.right));
    }
}
```

## 56 数组中只出现一次的两个数字***
题目：一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。  
思路：如果数组中只有一个数字只出现一次，我们从头到尾异或每个数字，那么最终的结果刚好是那个只出现一次的数字。而本题里数组中有两个数字只出现一次，如果能够将数组分为两部分，两部分中都只有一个数字只出现一次，那么就可以解决该问题了。  

代码实现过程：
1. 首先从头到尾异或每个数字，那么最终的结果就是这两个只出现一次的数字的异或结果；
2. 由于两个数不同，因此这个结果数字中一定有一位为1，把结果中第一个1的位置记为第n位。因为是两个只出现一次的数字的异或结果，所以这两个数字在第n位上的数字一定是1和0。
3. 接下来我们根据数组中每个数字的第n位上的数字是否为1来进行分组，恰好能将数组分为两个都只有一个数字只出现一次的数组，对两个数组从头到尾异或，就可以得到这两个数了。

```Java
//num1,num2分别为长度为1的数组。传出参数
//将num1[0],num2[0]设置为返回结果
public class Solution {
    public void FindNumsAppearOnce(int[] array, int[] num1, int[] num2) {
        // 边界判断
        if (array == null || array.length <= 1) {
            return;
        }
        // 先对数组中所有元素进行异或，最后结果是两个只出现一次的数字的异或。
        int resultExclusiveOR = 0;
        for (int i = 0; i < array.length; i++) {
            resultExclusiveOR ^= array[i];
        }
        // 找出异或结果的二进制表示，从右往左第一个为1的位置
        int indexOf1 = 0;
        while (((resultExclusiveOR&1) == 0) && indexOf1 <= 4*8) {
            resultExclusiveOR = resultExclusiveOR>>1;
            indexOf1++;
        }
        
        num1[0] = 0;
        num2[0] = 0;
        // 将原数组按照第 indexOf1 位是否是1进行分组
        for (int i = 0; i < array.length; i++) {
            if ( ((array[i]>>indexOf1)&1) == 1 ) {
                num1[0] ^= array[i];
            }else {
                num2[0] ^= array[i];
            }
        }
    }
}
```
收获：
1. 当一个数字出现两次（或者偶数次）时，用异或^ 可以进行消除。**一定要牢记异或的这个功能！**  
2. 将一组数字分为两组，可以根据某位上是否为1来进行分组，即根据和1相与（&1）的结果来进行分组。
3. 判断某个数x的第n位（如第3位）上是否为1，
	1）通过 x&00000100 的结果是否为0 来判断。（**不能根据是否等于1来判断**）
	2）通过（x>>3)&1 是否为0 来判断
4. 将某个数x右移m位，一定要写成 x=x>>m；而不能只写成 x>>m；这个语句

## 56-2 数组中唯一只出现一次的数字
题目：在一个数组中除了一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。  
思路：这道题中数字出现了三次，无法像56-1数组中只出现一次的两个数字一样通过利用异或位运算进行消除相同个数字，但是仍然可以沿用位运算的思路。将所有数字的二进制表示的对应位都加起来，如果某一位能被三整除，那么只出现一次的数字在该位为0；反之，为1。
```Java
public class FindNumAppearOnce {
    public int findNumAppearOnce(int[] array) {
        // 边界条件
        if (array == null || array.length <= 0) {
            throw new RuntimeException();
        }
        // 数组中各个数字的二进制表示，对应位上相加
        // bitSum数组存储规则，二进制的最低位和bitSum[31]对应
        int[] bitSum = new int[32];
        for (int i = 0; i < 32; i++) {
            bitSum[i] = 0;
        }
        for (int i = 0; i < array.length; i++) {
            int bitMask = 1;
            // 依次取出当前数字的各位，加到bitSum中的对应位上
            for (int j = 31; j >= 0; j--) {
                int bit = array[i] & bitMask;//注意arr[i]&bitMask不一定等于1或者0，有可能等于00010000
                if (bit != 0) {// 注意这个判断条件
                    bitSum[j] += 1;
                }
                bitMask = bitMask<<1;
            }
        }

        int result = 0;
        for (int i = 0; i < 32; i++) {
            result = result<<1;// 注意移位操作不能放在后面，否则最高位就没了
            result += (bitSum[i]%3);
        }
        return result;
    }
}
```
收获：
1、通过number&bitMask的结果是否为0（不能用1判断），bitMask=1不断左移，可以将一个数的二进制存储到32位的数组中。  
```Java
int number=100;
int bitMask=1;
for(int j=31;j>=0;j--) {
    int bit=number&bitMask;  //注意arr[i]&bitMask不一定等于1或者0，有可能等于00010000
    if(bit!=0) // 这个判断条件不能错，只能是不等于零
        bits[j]=1;
    bitMask=bitMask<<1;
}
```
2、通过以下代码实现二进制转化为数字（注意左移语句的位置）：
```Java
int result=0;
for(int i=0;i<32;i++) {
    result=result<<1;
    result+=bits[i];
    //result=result<<1;  //不能放在后面，否则最前面一位就没了
}
```

## 57-1  和为s的数字
题目：输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。  
```Java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int[] array, int sum) {
        ArrayList<Integer> list = new ArrayList<>();
        // 边界条件
        if (array == null || array.length < 2) {
            return list;
        }
        int lastIndex = array.length-1;
        int right = lastIndex;// 右边的指针，用于指向sum-array[i]的差值所在的位置，但right的位置不一定就是差值
        int maxMul = Integer.MAX_VALUE;;
        for (int i = 0; i < array.length && i < right; i++) {
            int diff = sum - array[i];
            if (diff > array[lastIndex]) {
                continue;
            }
            while (diff <= array[right-1] && right > i) {
                right--;
            }
            // 这种情况下所有的情况就已经考虑完了
            if (right <= i) {
                break;
            }
            // 此时找到了和为s的两个数字
            if (array[right] == diff && maxMul > array[i]*array[right]) {
                list.clear();
                list.add(array[i]);
                list.add(array[right]);
                maxMul = array[i]*array[right];
            }
        }
        return list;
    }
}
```
简洁版代码：
思路：我们考虑到，如果一个数字比较小，那么另一个数字一定比较大，同时数字为递增排列；所以，我们设置两个指针，一个指针small从第一个数字（最小）出发，另一个指针big从最后一个数字（最大）出发：

+ 当small加big的和小于s时，只需要将small指向后一个数字（更大），继续判断；
+ 当small加big的和大于s时，只需要将big指向前一个数字（更小），继续判断；
+ 当small加big的和等于s时，求解完成。

由于是从两边往中间移动，所以不会有跳过的情况，时间复杂度为O(n)。  
```Java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int[] array, int sum) {
        ArrayList<Integer> list = new ArrayList<>();
        // 边界条件
        if (array == null || array.length < 2) {
            return list;
        }
        int left = 0;
        int right = array.length-1;
        int maxMul = Integer.MAX_VALUE;;
        while (left < right) {
            int diff = sum - array[left];
            if (array[right] == diff && maxMul > array[left]*array[right]) {
                list.clear();
                list.add(array[left]);
                list.add(array[right]);
                maxMul = array[left]*array[right];
                left++;
            }else if (array[right] > diff) {
                right--;
            }else {
                left++;
            }
        }
        return list;
    }
}
```
## 57-2  和为s的连续正数序列
题目：小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!  
第一版：暴力法。  
```Java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        // 边界条件
        if (sum < 3) {
            return res;
        }
        
        for (int i = 1; i <= (sum>>1); i++) {
            ArrayList<Integer> list = new ArrayList<>();
            int start = i;
            int newSum = sum;
            while (newSum-start >= 0) {
                list.add(start);
                newSum -= start;
                start++;
            }
            if (newSum == 0) {
                res.add(list);
            }
        }
        return res;
    }
}
```
第二版：两个指针。  
思路：使用两个指针small和big分别代表序列的最大值和最小值。令small从1开始，big从2开始。
+ 当从small到big的序列的和小于s时，增加big，使序列包含更多数字；（记得更新序列之和）
+ 当从small到big的序列的和大于s时，增加small，使序列去掉较小的数字；（记得更新序列之和）
+ 当从small到big的序列的和等于s时，此时得到一个满足题目要求的序列，输出，然后继续将small增大，往后面找新的序列。

序列最少两个数字，因此，当small到了s/2时，就可以结束判断了。  

注意：边界条件；small的范围；small++和big++的位置。
```Java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        // 边界条件
        if (sum < 3) {
            return res;
        }
        
        int small = 1;
        int big = 2;
        int curSum = small+big;
        while (small <= (sum>>1)) {
            if (curSum == sum) {
                ArrayList<Integer> sequence = new ArrayList<>();
                for (int i = small; i <= big; i++) {
                    sequence.add(i);
                }
                res.add(sequence);
                curSum -= small;
                small++;
            }
            if (curSum > sum) {
                curSum -= small;
                small++;
            }
            if (curSum < sum) {
                big++;
                curSum += big;
            }
        }
        return res;
    }
}
```
LeetCode上的这一题，返回值不同，做法值得借鉴：
```Java
class Solution {
    public int[][] findContinuousSequence(int target) {
        int i = 1; 
        int j = 1;
        int sum = 0;// [i, j)的区间，所有此时sum为零
        List<int[]> list = new ArrayList<>();
        while (i <= (target>>1)) {
            if (sum < target) {
                sum += j;
                j++;
            }else if (sum > target) {
                sum -= i;
                i++;
            }else {
                int[] seq = new int[j-i];
                for (int k = i; k < j; k++) {
                    seq[k-i] = k;
                }
                list.add(seq);
                // 记录完只一次子序列之后继续查找，这里自己写的时候又忘掉了
                sum -= i;
                i++;
            }
        }
        return list.toArray(new int[list.size()][]);
    }
}
```
**知识点：List的toArray()方法和toArray(T[] a)方法**。  
这两个方法都是将列表List中的元素导出为数组，不同的是，toArray()方法导出的是Object类型数组，而toArray[T[] a]方法导出的是指定类型的数组。

+ toArray()返回的是一个新的数组对象。List的源码中toArray()方法只有一个声明，ArrayList中的实现源码中toArray()调用了Arrays工具类的copyOf()方法，所以**toArray()本质上就是直接调用的Arrays.copyOf()方法**。  
+ List接口的toArray(T[] a)方法会返回指定类型（必须为list元素类型的父类或本身）的数组对象。
	+ 如果a.length小于list元素个数就直接调用Arrays的copyOf()方法进行拷贝并且返回新数组对象，新数组中也是装的list元素对象的引用；
	+ 否则先调用System.arraycopy()将list元素对象的引用装在a数组中，如果a数组还有剩余的空间，则在a[size]放置一个null，size就是list中元素的个数，这个null值可以使得toArray(T[] a)方法调用者可以判断null后面已经没有list元素了。

System.arraycopy() 和 Arrays.copyOf()两者之间的区别：  
两者的区别在于，Arrays.copyOf()不仅仅只是拷贝数组中的元素，在拷贝元素时，会创建一个新的数组对象。而System.arrayCopy只拷贝已经存在数组元素。  
Arrays.copyOf()源码如下：
```java
public static int[] copyOf(int[] original, int newLength) { 
    int[] copy = new int[newLength]; 
    System.arraycopy(original, 0, copy, 0, Math.min(original.length, newLength)); 
    return copy; 
}
```

参考资料：  
[深入理解List的toArray()方法和toArray(T[] a)方法](https://blog.csdn.net/mucaoyx/article/details/86005283)  
[Java中System.arraycopy() 和 Arrays.copyOf()两者之间的区别](https://www.cnblogs.com/yongdaimi/p/5995414.html)  

第三版：数学分析法。  
思路：对于一个长度为n的连续序列，如果它们的和等于s，有：  
+ 当n为奇数时，s/n恰好是连续序列最中间的数字，即n满足 ```(n&1)==1 && s%n==0```
+ 当n为偶数时，s/n恰好是连续序列中间两个数字的平均值，小数部分为0.5，即n满足 ```(n&1)==0 && (s%n)*2==n```

得到满足条件的n后，相当于得到了序列的中间数字s/n，所以可以得到第一个数字为``` (s / n) - (n - 1) / 2```，结合长度n可以得到所有数字。  

此外，在什么范围内找n呢？我们知道n至少等于2，那至多等于多少？n最大时，序列从1开始，根据等差数列的求和公式根据等差数列的求和公式：```S = (1 + n) * n / 2```，可以得到n应该小于sqrt(2s)，所以只需要从n=2到sqrt(2s)来判断满足条件的n，继而输出序列。

参考资料： [丁满历险记的题解](https://www.nowcoder.com/questionTerminal/c451a3fd84b64cb19485dad758a55ebe)  

```Java
class Solution {
    public int[][] findContinuousSequence(int target) {
        List<int[]> list = new ArrayList<>();
        if (target <= 0) {
            return list.toArray(new int[0][]);
        }
        for (int n = (int)Math.sqrt(2*target); n >= 2; n--) {
            if ( ((n&1)==1 && target%n==0) || ((n&1)==0 && (target%n)*2==n) ) {
                // 找到了满足条件的长度n
                int first = (target/n) - (n-1)/2;// 这个序列的第一个数
                int[] seq = new int[n];
                for (int k = 0; k < n; k++) {
                    seq[k] = k+first;
                }
                list.add(seq);
            }
        }
        return list.toArray(new int[0][]);
    }
}
```

## 58-1  翻转单词顺序
题目：输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。   
第一版：使用字符串的split()方法，有缺陷（含有多个空格，或者首尾有空格的情况）。

```Java
public class Solution {
    public String ReverseSentence(String str) {
        StringBuilder res = new StringBuilder();
        if (str == null || str.length() == 0) {
            return res.toString();
        }
        String[] strArray = str.split(" ");
        if (strArray.length == 0) {
            return str;
        }
        for (int i = strArray.length-1; i >= 0; i--) {
            res.append(strArray[i]);
            if (i != 0) {
                res.append(" ");
            }
        }
        return res.toString();
    }
}
```
第二版：牛客网中函数输入是字符串，此时就必须要创建一个数组保存子串或者字符。但是正确的输入应该是字符数组（与书本保持一致），题目的隐含条件是不能使用额外的空间，否则就失去了这道题目的意义。正确的思路如下：
> 首先实现翻转整个句子：只需要在首尾两端各放置一个指针，交换指针所指的数字，两端指针往中间移动即可。之后根据空格的位置，对每个单词使用同样的方法翻转即可。

```Java
public class ReverseSequence {
    public String reverseSequenc(char[] chars) {
    	// 边界条件判断
        if (chars == null || chars.length <= 0) {
            return String.valueOf(chars);
        }
        // 首先翻转整个句子
        reverse(chars, 0, chars.length-1);

        // 然后翻转每个单词
        int start = 0;
        int end = 0;
        while (start < chars.length) {
            while (end < chars.length && chars[end] != ' ') {
                end++;
            }
            reverse(chars, start, end-1);
            end++;
            start = end;
        }
        return String.valueOf(chars);
    }

    private void reverse(char[] chars, int start, int end) {
        while (start < end) {
            char temp = chars[start];
            chars[start] = chars[end];
            chars[end] = temp;
            start++;
            end--;
        }
    }

    public static void main(String[] args) {
        ReverseSequence demo = new ReverseSequence();
        char[] chars = {'I',' ','a','m',' ','a',' ','s','t','u','.'};
        char[] chars2 = {'a',' ',' ','b',' ','c'};
        System.out.println(demo.reverseSequenc(chars2));
    }
}
```
## 58-2  左旋转字符串
题目：字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如输入字符串"abcdefg"和数字2，该函数将返回左旋转2位得到的结果"cdefgab"。
思路：字符串的移动 → 字符串的翻转

```Java
public class Solution {
    public String LeftRotateString(String str,int n) {
        if (str == null || str.length() == 0 || n < 0 || n > str.length()) {
            return "";
        }
        char[] chars = str.toCharArray();
        // 先翻转整个字符数组
        reverse(chars, 0, chars.length-1);
        // 再对两部分，即chars.length-n个字符和后n个字符，分别进行翻转
        reverse(chars, 0, chars.length-1-n);
        reverse(chars, chars.length-n, chars.length-1);
        return String.valueOf(chars);
    }
    // 翻转chars数组中[start, end]范围内的字符
    private void reverse(char[] chars, int start, int end) {
        while (start < end) {
            char temp = chars[start];
            chars[start] = chars[end];
            chars[end] = temp;
            start++;
            end--;
        }
    }
}
```
## 59-1  滑动窗口的最大值
**题目**：给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。  
**思路**：  
第一版：自己写的代码。  

```Java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> maxInWindows(int[] num, int size){
        ArrayList<Integer> list = new ArrayList<>();
        if (num == null || num.length <= 0 || size <= 0 || size > num.length) {
            return list;
        }
        int start = 0;
        int end = size-1;
        int localMax = getMax(num,start,end);
        list.add(localMax);
        while (end < num.length-1) {
            start++;
            end++;
            // 最大值被移出去了
            if (num[start-1] == localMax) {
                localMax = getMax(num,start,end);
            }
            if (num[end] > localMax) {
                localMax = num[end];
            }
            list.add(localMax);
        }
        return list;
    }
    
    private int getMax(int[] num, int start, int end) {
        int localMax = num[start];
        for (int i = start+1; i <= end; i++) {
            if (num[i] > localMax) {
                localMax = num[i];
            }
        }
        return localMax;
    }
}
```
第二版：双端队列。
**思路**：我们考虑把每个可能成为最大值的数字记录下来，就可以快速的得到最大值。**建立一个两端开口的队列，放置所有可能是最大值的数字（存放的其实是对应的下标），且最大值位于队列开头**。从头开始扫描数组，有如下几种情况：

+ 如果遇到的数字比队列中所有的数字都大，那么它就是最大值，其它数字不可能是最大值了，将队列中的所有数字清空，放入该数字，该数字位于队列头部；
+ 如果遇到的数字比队列中的所有数字都小，那么它还有可能成为之后滑动窗口的最大值，放入队列的末尾；
+ 如果遇到的数字比队列中最大值小，最小值大，那么比它小的数字不可能成为最大值了，删除较小的数字，放入该数字。
+ 由于滑动窗口有大小，因此，队列头部的数字如果其下标离滑动窗口末尾的距离大于窗口大小，那么也删除队列头部的数字。

```Java
import java.util.ArrayList;
import java.util.ArrayDeque;
public class Solution {
    public ArrayList<Integer> maxInWindows(int[] num, int size){
        ArrayList<Integer> list = new ArrayList<>();
        if (num == null || num.length <= 0 
            || size <= 0 || size > num.length) {
            return list;
        }
        ArrayDeque<Integer> indexDeque = new ArrayDeque<>();
        // 为什么要有这个for循环，因为在size-1的时候才会进行添加最大值
        for (int i = 0; i < size-1; i++) {
            while(!indexDeque.isEmpty() 
                  && num[i] > num[indexDeque.getLast()]) {
                indexDeque.removeLast();
            }
            indexDeque.addLast(i);
        }
        
        for (int i = size-1; i < num.length; i++) {
            while (!indexDeque.isEmpty() 
                   && num[i] > num[indexDeque.getLast()]) {
                indexDeque.removeLast();
            }
            if (!indexDeque.isEmpty() 
               && i-indexDeque.getFirst() >= size) {
                indexDeque.removeFirst();
            }
            indexDeque.addLast(i);
            list.add(num[indexDeque.getFirst()]);
        }
        return list;
    }
}
```
## 59-2  队列的最大值
题目：请定义一个队列并实现函数max得到队列里的最大值，要求函数max、push_back和pop_front的时间复杂度都是O(1)。  
思路：利用一个双端队列来存储当前队列里的最大值以及之后可能的最大值。在定义题目要求功能的队列时，除了定义一个队列data存储数值，还需额外用一个队列maxmium存储可能的最大值；此外，还要定义一个数据结构，用于存放数据以及当前的index值，用于删除操作时确定是否删除maxmium中最大值。  
```Java
import java.util.ArrayDeque;

public class QueueWithMax {

    private class InternalData {
        private int number;
        private int index;
        public InternalData(int number, int index) {
            this.number = number;
            this.index = index;
        }
    }

    private ArrayDeque<InternalData> dataQueue = new ArrayDeque<>();
    private ArrayDeque<InternalData> maxQueue = new ArrayDeque<>();

    private int curIndex = 0;

    public void push_back(int number) {
        InternalData data = new InternalData(number, curIndex);
        dataQueue.addLast(data);

        while (!maxQueue.isEmpty() && number > maxQueue.getLast().number) {
            maxQueue.removeLast();
        }
        maxQueue.addLast(data);
        curIndex++;
    }

    public void pop_front() {
        if (dataQueue.isEmpty()) {
            throw new IllegalArgumentException("Queue is empty.");
        }
        InternalData delData = dataQueue.removeFirst();
        if (delData.index == maxQueue.getFirst().index) {
            maxQueue.removeFirst();
        }
    }

    public int max() {
        if (maxQueue.isEmpty()) {
            throw new IllegalArgumentException("Queue is empty.");
        }
        return maxQueue.getFirst().number;
    }
}
```
## 60 n个骰子的点数
题目：把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。  
思路：基于循环求骰子点数，时间性能好。
+ 用数组存放每种骰子点数和出现的次数。令数组中下标为n的元素存放点数和为n的次数。
+ 我们设置循环，每个循环多投掷一个骰子，假设某一轮循环中，我们已知了各种点数和出现的次数；在下一轮循环时，我们新投掷了一个骰子，那么此时点数和为n的情况出现的次数就等于上一轮点数和为n-1,n-2,n-3,n-4,n-5,n-6的情况出现次数的总和。即点数之和f(n)=f(n-1)+…+f(n-6)。
+ 从第一个骰子开始，循环n次，就可以求得第n个骰子时各种点数和出现的次数。我们这里用两个数组来分别存放本轮循环与下一轮循环的各种点数和出现的次数，不断交替使用。
```Java
import java.text.NumberFormat;
public class DicesProbability {
    private static final int MAX_VALUE = 6;

    public static void printProbability(int number) {
        if (number <= 0) {
            return;
        }
        // 保存骰子点数的数组，并初始化
        // [2]代表用两个数组交替保存
        // [number*maxValue+1]开这么大长度为了点数和数组下标对应，数组值就是该点数出现的总次数。
        int[][] pointNum = new int[2][MAX_VALUE*number+1];
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < MAX_VALUE*number+1; j++) {
                pointNum[i][j] = 0;
            }
        }
        // 考虑第一个骰子
        for (int i = 1; i <= MAX_VALUE; i++) {
            pointNum[0][i] = 1;
        }

        int flag = 1;// 指向二维数组中的哪一行，上面代码考虑了第一个筛子，下面考虑两个筛子，指向第二行
        for (int curNumber = 2; curNumber <= number; curNumber++) {
            // 当前有curNumber个骰子，所以点数至少是curNumber(每个骰子都为1)
            // 那么将小于curNumber的点数都清零
            for (int i = 0; i < curNumber; i++) {
                pointNum[flag][i] = 0;
            }
            // 当前curNumber个骰子的点数范围
            for (int i = curNumber; i <= curNumber*MAX_VALUE; i++) {
                for (int j = 1; j <= MAX_VALUE && j < i; j++) {
                    pointNum[flag][i] += pointNum[1-flag][i-j];
                }
            }
            flag = 1-flag;
        }

        int totalP = (int) Math.pow(MAX_VALUE, number);
        for (int i = number; i <= number*MAX_VALUE; i++) {
            double ratio = (double) pointNum[1-flag][i]/totalP;
            NumberFormat format = NumberFormat.getPercentInstance();
            format.setMaximumFractionDigits(8);
            System.out.println("点数和为 " + i + " 的概率为： "+format.format(ratio));
        }
    }

    public static void main(String[] args) {
        for(int i=0;i<=3;i++) {
            System.out.println("-----骰子数为"+i+"时-----");
            printProbability(i);
        }
        System.out.println("-----骰子数为"+11+"时-----");
        printProbability(11);
    }
}
```
## 61 扑克牌中的顺子
题目：从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王可以看成任意数字。  
思路：输入为大小等于5的数组（大小王记为0），输出为布尔值。具体步骤如下：
1. 首先对5张牌进行排序；
2. 遍历算出0的个数；
3. 算出相邻数字的空缺总数，比如3和5之间空缺一张，5和6之间不空缺；
4. 如果0的个数大于等于空缺总数，说明连续，反之不连续；

注意：记得判断相邻数字是否相等，如果有出现相等，说明不是顺子。
```Java
import java.util.Arrays;
public class Solution {
    public boolean isContinuous(int[] numbers) {
        // 边界条件
        if (numbers == null || numbers.length <= 0) {
            return false;
        }
        // 先对数组排序，大小王是零，排在前面
        Arrays.sort(numbers);
        int timeOf0 = 0;
        int timeOfGap = 0;
        // 获取0的个数
        for (int i = 0; i < numbers.length; i++) {
            if (numbers[i] == 0) {
                timeOf0++;
            }
        }
        // 获取缺失数字的个数
        int small = timeOf0;
        int big = timeOf0+1;
        while (big < numbers.length) {
            // 邻居相等，肯定不是顺子
            if (numbers[small] == numbers[big]) {
                return false;
            }
            timeOfGap += numbers[big++]-numbers[small++]-1;
        }
        if (timeOf0 >= timeOfGap) {
            return true;
        }
        return false;
    }
}
```
## 62 圆圈中最后剩下的数字
**题目**：0, 1, …, n-1这n个数字排成一个圆圈，从数字0开始每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
**思路：数学推导规律。**   
n个数字的圆圈，不断删除第m个数字，我们把最后剩下的数字记为f(n,m)。n个数字中第一个被删除的数字是(m-1)%n， 我们记作k，k=(m-1)%n。那么剩下的n-1个数字就变成了：0,1,……k-1,k+1,……,n-1，我们把下一轮第一个数字排在最前面，并且将这个长度为n-1的数组映射到0~n-2。  
```
原始数字：k+1,……,   n-1,       0,    1,……k-1
映射数字：0    ,……,n-k-2, n-k-1, n-k,……n-2
```
把映射数字记为x，原始数字记为y，那么映射数字变回原始数字的公式为：  
```
y=(x+k+1)%n
```
在映射数字中，n-1个数字，不断删除第m个数字，由定义可以知道，最后剩下的数字为f(n-1,m)。我们把它变回原始数字，由上一个公式可以得到最后剩下的原始数字是（f(n-1,m)+k+1)%n，而这个数字就是也就是一开始我们标记为的f(n,m)，所以可以推得递归公式如下：  
```
f(n,m) =（f(n-1,m)+k+1)%n
```
将k=(m-1)%n代入，化简得到：
```
f(n,m) =（f(n-1,m)+m)%n
f(1,m) = 0
```
代码中可以采用循环或者递归的方法实现该递归公式。时间复杂度为O(n)，空间复杂度为O(1)。  
```Java
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if (n < 1 || m < 1) {
            return -1;
        }
        int last = 0;
        for (int i = 1; i <= n; i++) {
            last = (last+m)%i;
        }
        return last;
    }
}
```
## 63 股票的最大利润
**题目**：假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖交易该股票可能获得的利润是多少？例如一只股票在某些时间节点的价格为{9, 11, 8, 5,7, 12, 16, 14}。如果我们能在价格为5的时候买入并在价格为16时卖出，则能收获最大的利润11。  
**思路**：遍历每一个数字，并保存之前最小的数字，两者差最大即为最大利润。  
```Java
public class MaxProfit {
    public static int maxProfit(int[] arr) {
        if (arr == null || arr.length < 2) {
            throw new IllegalArgumentException("Input is illegal.");
        }
        int min = arr[0];
        int maxDiff = arr[1] - min;
        for (int i = 1; i < arr.length; i++) {
            // 保存当前位置i之前的最小值
            if (arr[i-1] < min) {
                min = arr[i-1];
            }
            if (arr[i]-min > maxDiff) {
                maxDiff = arr[i]-min;
            }
        }
        return maxDiff;
    }
}
```
## 64 求1+2+...+n
题目：求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。  
思路：利用逻辑操作&&或者||的短路特性来代替if语句，递归代替循环操作。  
```Java
public class Solution {
    public int Sum_Solution(int n) {
        int sum = n;
        boolean flag = (n > 1) && ((sum += Sum_Solution(n-1))>0);
        // 或者这样写
        //boolean flag = (n == 1) || ((sum += Sum_Solution(n-1))>0);
        return sum;
    }
}
```
## 65 不用加减乘除做加法
题目：写一个函数，求两个整数之和，要求在函数体内不得使用+、-、\*、/四则运算符号。  
思路：对数字做运算，除了四则运算外，只剩下位运算了。根据一般情况下的加法步骤，设计如下：  
1. 不考虑进位对每一位相加：1加0，0加1都等于1，而0加0，1加1等于0，所以使用异或^操作；
2. 计算进位：只有1加1产生进位，所以采用位与&操作，再左移1位；
3. 将和与进位相加，即重复前两步操作。结束判断为进位为0。
```Java
public class Solution {
    public int Add(int num1,int num2) {
        while (num2 != 0) {
            int sum = num1 ^ num2;
            int carry = (num1&num2)<<1;
            num1 = sum;
            num2 = carry;
        }
        return num1;
    }
}
```

## 66 构建乘积数组
**题目**：给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0] * A[1] * ... * A[i-1] * A[i+1] * ... * A[n-1]。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2]）  
**思路**：考虑到计算每个B[i]时都会有重复，思考B[i]之间的联系，找出规律，提高效率。  
![](.\images\乘积数组.png)  
<center>乘积数组示意图</center>
如上图所示，可以发现：
+ B[i]的左半部分(红色部分)和B[i-1]有关（将B[i]的左半部分乘积看成C[i]，有C[i]=C[i-1] * A[i-1]）；
+ B[i]的右半部分(紫色部分)与B[i+1]有关（将B[i]的右半部分乘积看成D[i]，有D[i]=D[i+1] * A[i+1]）；

因此我们先从0到n-1遍历，计算每个B[i]的左半部分；  然后定义一个变量temp代表右半部分的乘积，从n-1到0遍历，令B[i] * =temp，而每次的temp与上次的temp关系即为temp * =A[i+1]。  

```Java
import java.util.ArrayList;
public class Solution {
    public int[] multiply(int[] A) {
        if (A == null || A.length < 2) {
            return new int[]{};
        }
        int n = A.length;
        int[] B = new int[n];
        B[0] = 1;
        // 首先计算前半部分
        for (int i = 1; i < n; i++) {
            B[i] = B[i-1]*A[i-1];
        }
        int temp = 1;
        for (int i = n-2; i >= 0; i--) {
            temp *= A[i+1];
            B[i] *= temp;
        }
        return B;
    }
}
```
## 67 把字符串转换为整数
题目：将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。  
```Java
public class Solution {
    public int StrToInt(String str) {
        // 边界条件
        if (str == null || str.length() == 0) {
            return 0;
        }
        int len = str.length();
        long sum = 0;
        boolean plus = true;
        
        for (int i = 0; i <= len-1; i++) {
            // 数值的符号处理
            if (i == 0 && (str.charAt(0)=='+' )) {
                plus = true;
            } else if (i == 0 && (str.charAt(0)=='-' )) {
                plus = false;
            } else {
                // 是否是数字
                int diff = str.charAt(i) - '0';
                if (diff<0 || diff>9) {
                    return 0;
                }
                
                sum = (plus == true) ? sum*10+diff : sum*10-diff;
                // 判断是否越界
                if ( (plus && sum > 0x7fffffff)
                   || (!plus && sum < 0x80000000) ) {
                    return 0;
                }
            }
        }
        return (int)sum;
    }
}
```

## 68 树中两个节点的最低公共祖先
题目：输入两个树结点，求它们的最低公共祖先。  
思路：该题首先要和面试官确定是否为二叉树，得到肯定答复后，还要确定是否为二叉搜索树，是否有父指针，或者仅仅是普通二叉树。
1. 树为二叉搜索树时，最低公共祖先结点的大小在两个树结点大小的中间。
2. 树为普通树时，使用遍历将子结点的信息**往上传递**。在左右子树中进行查找是否存在两个树结点，如果两个树结点分别在左右子树上，说明该根结点就是它们的最低公共祖先。

总结：这道题目对题目中的树考虑不够周全。对树的遍历不够熟悉。
```Java
public class LowestCommonParent {
    private class TreeNode{
        private int val;
        private TreeNode left;
        private TreeNode right;
        public TreeNode(int val) {
            this.val = val;
        }
    }
	// 二叉搜索树：利用大小关系即可
    public TreeNode getLowestCommonParentBST(TreeNode root, TreeNode node1, TreeNode node2) {
        while (true) {
            // root为空的判断条件放到里面来
            if (root == null) {
                return root;
            }
            if (root.val < node1.val && root.val < node2.val) {
                root = root.right;
            }else if (root.val > node1.val && root.val > node2.val) {
                root = root.left;
            }else {// root的值在node1和node2之间
                return root;
            }
        }
    }
	// 普通二叉树：将下面节点的信息利用递归向上传递
    public TreeNode getLowestCommonParent(TreeNode root, TreeNode node1, TreeNode node2) {
        if (root == null || root == node1 || root == node2) {
            return root;
        }
        TreeNode left = getLowestCommonParent(root.left, node1, node2);
        TreeNode right = getLowestCommonParent(root.right, node1, node2);
        return left==null ? right : right==null ? left : root;
    }
}
```
