###### 1.翻转二叉树

> 输入：
> 	4
>    /   \
>   2     7
>  / \   / \
> 1   3 6   9
> 输出：
> 	4
>    /   \
>   7     2
>  / \   / \
> 9   6 3   1

​	有三种实现方式：栈、队列、递归。

```java
class Solution{
    public TreeNode invertTree(TreeNode root){
        if(root == null){
            return null;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            final TreeNode node = stack.pop();
            final TreeNode left = node.left;
            node.left = node.right;
            node.right = left;
            if(node.left != null){
                stack.push(node.left);
            }
            if(node.right != null){
                stack.push(node.right);
            }
        }
        return root;
    }
}//栈实现
```

```java
class Solusion{
    public TreeNode invertTree(TreeNode root){
        if(root == null){
            return null;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode node = q.poll();
            TreeNode left = node.left;
            node.left = node.right;
            node.right = left;
            if(node.left != null){
                q.offer(node.left);
            }
            if(node.right != null){
                q.offer(node.right);
            }
        }
        return root;
    }
}//队列实现
```

```java
class Solution{
    public TreeNode invertTree(TreeNode node){
        if(node == null){
            return null;
        }
        TreeNode left = node.left;
        node.left = node.right;
        node.right = left;
        invertTree(node.left);
        invertTree(node.right);
        return node;
    }
}//递归实现
```

###### 2.用两个栈实现一个队列。

​	队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

```java
class CQueue {
    Stack<Integer> stack1;
    Stack<Integer> stack2;

    public CQueue() {
        stack1 = new Stack<Integer>();
        stack2 = new Stack<Integer>();
    }

    public void appendTail(int value) {
        stack1.push(value);
    }

    public int deleteHead() {
        if (!stack2.isEmpty()){
            return stack2.pop();
        } else if (stack1.isEmpty()){
            return -1;
        }else {
            while (!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
            return  stack2.pop();
        }
    }
}
```

###### *3.找出数组中重复的数字	(hashset)

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {	//将数组元素添加进hashset; add()方法添加重复元素返回false
                repeat = num;
                break;
            }
        }
        return repeat;
    }
}
```

###### <font color="#FF0000">*4.二维数组中的查找</font>

 [1,   4,  7, 11, 15],			每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。
  [2,   5,  8, 12, 19],			完成一个函数，输入一个二维数组和一个整数，判断数组中是否含有该整数。
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        boolean flag = false;
        int i = matrix.length - 1;
        int j = 0;
        while(i >= 0 && j <= matrix[0].length - 1){
            if(matrix[i][j] > target) i--;
            else if(matrix[i][j] < target) j++;
            else	flag = true;
        }
        return flag;
    }
}
```

###### 5.替换字符串空格

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder ns = new StringBuilder();
        for(char c : s.toCharArray()){	//''内表示char类型，""内表示String类型
            if(c == ' ')	ns.append("%20");
            else	ns.append(c);
        }
        return ns.toString();
    }
}
```

###### *6.斐波那契数列	&&	青蛙跳台阶问题（一次跳1阶或2阶）

```java
class Solution {
    public int fib(int n) {
        int a = 0, b = 1, sum = 0;
        for(int i = 0; i < n; i++){	//递归超时，采用动态规划方法
            sum = (a + b) % 1000000007;
            a = b;
            b = sum;
        }
        return a;
    }
}
```

###### 7.二分查找

###### 8.二进制中1的个数

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        while(n != 0){
           if((n & 1) == 1)count++;	//&的优先级不如==
           n >>>= 1;
        }
        return count;
    }
}//左移位运算符（<<）、右移位运算符（>>>）、带符号的右移位运算符（>>）
```

###### <font color="#FF0000">9.链表翻转</font>

```java
class Solution {
    public static ListNode reverseList(ListNode head) {
        if(head == null)return null;
        ListNode nhead = new ListNode(0);
        nhead.next = head;
        ListNode pre = head;
        ListNode cur = head.next;
        while(cur != null){
            pre.next = cur.next;
            cur.next = nhead.next;
            nhead.next = cur;
            cur = pre.next;
        }
        return nhead.next;
    }
}
```

###### <font color="#FF0000">10.合并两个已排序链表</font>

```java
class Solution {
        public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
            ListNode head = new ListNode(-1), l3 = head;
            if(l1 == null && l2 == null)return null;
            while(l1 != null && l2 != null){
                if(l1.val < l2.val){
                    l3.next = l1;
                    l1= l1.next;
                }esle if(l1.val >= l2.val){
                    l3.next = l2;
                    l2= l2.next;
                }
                l3 = l3.next;
            }
            l3.next = (l1 != null) ? l1 : l2;
            return head.next;
        }
}
```

###### <font color="#FF0000">*11.判断对称二叉树</font>

> 二叉树 [1,2,2,3,4,4,3] 是对称的。
>
> ​	1
>   / \
>  2  2
>  / \ / \
> 3  4 4  3
>
> [1,2,2,null,3,null,3] 则不是镜像对称的:
>
> ​	1
>   / \
>  2  2
>   \  	\
>   3   	3

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        else return isEqual(root.left, root.right);
    }
    /*
     *任意满足L.val == R.val的L、R节点还需满足：
     *L.left.val == R.right.val; L.right.val == L.left.val
    */
    public boolean isEqual(TreeNode L, TreeNode R){
        if(L == null && R == null)	return true;
        if(L == null || R == null || L.val != R.val)	return false;
        return recur(L.left, R.right) && recur(L.right, R.left);	//需同时满足L的左节点等于R的右节点；L的右节点等于R的左节点
    }
}
```

<font color="#FF0000">*12.顺时针打印二维矩阵</font>

> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```java
public static int[] spiralOrder(int[][] matrix) {
        int length = matrix[0].length * matrix.length;
        int[] newMatrix = new int[length];
        int xia = matrix.length - 1, shang = 0, you = matrix[0].length - 1, zuo = 0, n = 0;//定义上下左右边界
        if (matrix.length == 0) return new int[0];
    /*
      *四个方向循环
      */
        while (true) {
            for (int j = zuo; j <= you; j++)  newMatrix[n++] = matrix[shang][j];	//从左边界往右不超过右边界
            if (++shang > xia)    break;	//上边界大于下边界跳出
            for (int i = shang; i <= xia; i++)  newMatrix[n++] = matrix[i][you];	//从上边界往下不超过下边界
            if (--you < zuo)    break;	//右边界小于左边界跳出
            for (int j = you; j >= zuo; j--) newMatrix[n++] = matrix[xia][j];	//从右边界往左不超过左边界
            if (--xia < shang)  break;	//下边界大于上边界跳出
            for (int i = xia; i >= shang; i--)   newMatrix[n++] = matrix[i][zuo];	//从下边界往上不超过上边界
            if (++zuo > you)    break;	//左边界大于右边界跳出
        }
        return newMatrix;
    }
```

13.包含min函数的栈	<font color="#FF0000">要求:min()以O(1)运行	(双栈)</font>

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```

```java
public class MinStack {
    Stack<Integer> stackA, stackB;
    public MinStack() {
        stackA = new Stack<>();
        stackB = new Stack<>();
    }

    public void push(int x) {
        stackA.push(x);
        if (stackB.empty() || stackB.peek() > x)    stackB.push(x);
    }

    public void pop() {
        if (stackA.pop() == stackB.peek())
            stackB.pop();
    }

    public int top() {
        return stackA.peek();
    }

    public int min() {
        return stackB.peek();
    }
}
```

<font color="#FF0000">13.二叉树的广度优先搜索	(队列)</font>

```java
public static List<TreeNode> TreeOrder(TreeNode root){
        if (root == null)   return null;
        Queue<TreeNode> queue = new LinkedList<>();
        List<TreeNode> list = new LinkedList<>();
        queue.offer(root);	//根节点入队
    //队列实现
        while (!queue.isEmpty()){
            TreeNode temp = queue.poll();	//出队
            list.add(temp);
            if (temp.left != null)  queue.offer(temp.left);	//左子树不为空则入队
            if (temp.right != null) queue.offer(temp.right);	//右子树不为空则入队
        }
        return list;
    }
```

<font color="#FF0000">14.二叉树的广度优先搜索II</font>

```
给定二叉树：
 	3
   / \
  9  20
    /  \
   15   7
返回[[3],[9,20],[15,7]]
```

```java
public static List<List<Integer>> TreeOrder(TreeNode root){
        if (root == null)   return null;
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> list = new LinkedList<>();
        queue.offer(root);
       while (!queue.isEmpty()){
            List<Integer> l = new LinkedList<>();
            for (int i = queue.size(); i > 0; i--) {	//队列长度即为二叉树每行元素个数
                TreeNode temp = queue.poll();
                l.add(temp.val);
                if (temp.left != null) queue.offer(temp.left);
                if (temp.right != null) queue.offer(temp.right);
            }
            list.add(l);
        }
        return list;
    }
```

*15.数组中出现次数超过一半的数字	<font color="#FF0000">HashMap的运用</font>

```java
	public static int majorityElement(int[] nums) {
        int length = nums.length;
        Map<Integer,Integer> map = new HashMap<>();	//key = value, key存数组中数字，value存重复次数
        if (length == 0) return 0;
        for (int i = 0; i < length; i++){
            if (map.containsKey(nums[i])){	//若map包含nums[i]中数字
                int count = map.get(nums[i]);	//获取key对应value
                map.put(nums[i], ++count);	//更新对应关系
            }else map.put(nums[i], 1);	//否则直接新增
        }
        for (int i : map.keySet()){
            if (map.get(i) * 2 > length)    return i;
        }
        return 0;
    }	//另一种解法：先排序，数组中间的数就是超过一半的数
```

