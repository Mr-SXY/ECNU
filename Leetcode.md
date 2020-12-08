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
//本质上是前序遍历
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

###### <font color="#FF0000">9.链表首尾翻转</font>

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
            if(l1 == null && l2 == null)	return null;
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
        return isEqual(L.left, R.right) && isEqual(L.right, R.left);	//需同时满足L的左节点等于R的右节点；L的右节点等于R的左节点
    }
}
```

###### <font color="#FF0000">*12.顺时针打印二维矩阵</font>

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

###### 13.包含min函数的栈	<font color="#FF0000">要求:min()以O(1)运行	(双栈)</font>

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

###### <font color="#FF0000">13.二叉树的广度优先搜索	(队列)</font>

```java
public static List<TreeNode> TreeOrder(TreeNode root){
        if (root == null)   return null;
        Queue<TreeNode> queue = new LinkedList<>();
        List<TreeNode> list = new LinkedList<>();
        queue.offer(root);	//根节点入队

        while (!queue.isEmpty()){
            TreeNode temp = queue.poll();	//出队
            list.add(temp);
            if (temp.left != null)  queue.offer(temp.left);	//左子树不为空则入队
            if (temp.right != null) queue.offer(temp.right);	//右子树不为空则入队
        }
        return list;
    }
```

###### <font color="#FF0000">14.二叉树的广度优先搜索II</font>

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

###### *15.数组中出现次数超过一半的数字	<font color="#FF0000">HashMap的运用</font>

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

###### *16.数组最小的k个数	<font color="#FF0000">**大顶堆**</font>

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

```java
public int[] getLeastNumbers(int[] arr, int k) {
        int[] newArr = new int[k];
        if (arr.length == 0)    return new int[0];
        for (int gap = arr.length / 2; gap > 0; gap /= 2){	//shell排序
            for (int i = gap; i < arr.length; i++) {
                int temp = arr[i];
                int j = i;
                for (; j >= gap && temp < arr[j - gap]; j -= gap)
                    arr[j] = arr[j - gap];
                arr[j] = temp;
            }
        }
        for (int i = 0; i < k; i++)
            newArr[i] = arr[i];
        return newArr;
    }
```

###### *17.连续子数组的最大和	<font color="#FF0000">**动态规划**</font>

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

```java
public int maxSubArray(int[] nums) {
        int length = nums.length;
        if (length == 0)   return 0;
        int[] res = new int[length];	//res[]存储子数组和
        res[0] = nums[0];
        for (int i = 1; i < length; i++){
            if (res[i - 1] >= 0) res[i] = res[i - 1] + nums[i];	//res[i - 1]为正表示之前的子数组对后续有提升
            else res[i] = nums[i];	//res[i - 1]小于0，提升为负，重新确定子数组起始位置
        }
        int max = res[0];
        for (int i = 1; i < length; i++){
            if (res[i] > max)   max = res[i];
        }
        return  max;
    }
```

###### *18.第一个只出现一次的字符	**HashMap**

字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```

```java
public static char firstUniqChar(String s) {
        if (s == "")    return ' ';
        HashMap<Character,Integer> map = new HashMap<>();	//HasMap<Character,Boolean>更佳
        for (int i = 0; i < s.length(); i++){
            if (map.containsKey(s.charAt(i))) {
                int count = map.get(s.charAt(i));
                map.put(s.charAt(i), ++count);
            }
            else map.put(s.charAt(i),1);
        }
        for (char c : s.toCharArray()){	//HashMap无序，通过重新遍历String获取顺序
            if (map.get(c) == 1)    return c;	//返回第一个value为1的key
        }
        return ' ';	//若return 0，输入空串返回'\u0000'
    }
```

###### *19.找出两个链表的第一个公共节点

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p = headA;
        ListNode q = headB;
        int counta = 1, countb = 1;
    //遍历两个链表，得到各自长度
        while (p != null){
            counta++;
            p = p.next;
        }
        while (q != null){
            countb++;
            q = q.next;
        }
    //重新指回首地址
        p = headA;
        q = headB;
        if (counta > countb){
            for (int i = 0; i < counta - countb; i++)   p = p.next;
            while (p != null){
                p = p.next;
                q= q.next;
                if (p == q) return p; 	//不是值val相等，而是节点相等
            }
        }else {
            for (int i = 0; i < countb - counta; i++)   q = q.next;
            while (p != null){
                p = p.next;
                q= q.next;
                if (p == q) return p;
            }
        }
        return null;
    }
```

###### 20.二叉搜索树的第K大的节点

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

```java
public int kthLargest(TreeNode root, int k) {
        List<Integer> list = new LinkedList<>();
        list = TreetoList(root, list);
        return list.get(list.size() - k);
    }
/*
 *二叉搜索树的 *中序遍历* 即为递增序列，中序遍历倒序即为递减序列
 *用倒序更佳，即先TreetoList(root.right, list);	后TreetoList(root.left, list);
*/
    public List<Integer> TreetoList(TreeNode root, List<Integer> list){
        if (root != null){	//中序遍历
            TreetoList(root.left, list);
            list.add(root.val);
            TreetoList(root.right, list);
        }
        return list;
    }
```

###### <font color="#FF0000">*21.二叉树的深度</font>

```java
 public int maxDepth(TreeNode root) {
        if (root == null) return 0;	//递归出口，若节点为空返回深度为0
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return left > right ? left + 1 : right + 1;	//某节点的深度 等于 其左子树与右子树深度的最大值+1
    }
```

###### <font color="#FF0000">*22.平衡二叉树的判断</font>	<font color="#FF0000">**分治**</font>

```java
public boolean isBalanced(TreeNode root) {
        if (root == null)   return true;	//递归出口，若节点为空返回itrue
        return isBalanced(root.left) && isBalanced(root.right) && Math.abs(Depth(root.left) - Depth(root.right)) <= 1;
    }
/*递归套娃	**分治算法**
 *判断一棵树是否为AVL树：
 *1.其左子树和右子树均为AVL树；
 *2.该节点的左子树右子树的深度差不超过1。
 */
    public int Depth(TreeNode root){
        if (root == null)   return  0;
        int left = Depth(root.left);
        int right = Depth(root.right);
        return left > right ? left+1 : right+1;
    }
```

###### 23.和为s的两个数字

输入为一个**递增序列**。

```java
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]

//双指针一头一尾，大于target尾指针前移，小于头指针后移，相同break
```

###### <font color="#FF0000">*24.和为s的连续正数序列</font>	**滑动窗口法**

```java
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

```java
/*采用双指针移动
 *若指针区域内的和小于target，头指针前移；
 *若指针区域内的和大于target，尾指针前移；
  *若相等，加入新数组并加入链表，并且尾指针前移；
 */
public int[][] findContinuousSequence(int target) {
        int length = target / 2 + 1;	//由于要求连续，故不会超过target的一半
        List<int[]> res = new ArrayList<>();	//一维数组合并成二维采用ArrayList
        for (int tail = 0, head = 1; tail < head;){	//尾指针超过头指针跳出循环
            int[] newnum = new int[head-tail+1];
            int sum = (tail+head+2)*(head-tail+1)/2;	//和可用公式求得
            if (sum > target) tail++;
            else if (sum < target) head++;
            else {
                for (int j = tail,n = 0; j <= head; j++)
                    newnum[n++] = j+1;
                res.add(newnum);
                tail++;
            }
        }
        return res.toArray(new int[res.size()][]);	//采用toArray(T[])方法返回T[]
    }
```

###### *25.翻转单词顺序

```java
输入: "the sky is blue"
输出: "blue is sky the"

输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

```java
public String reverseWords(String s) {
    	s = s.trim();
    /*	trim()实现
      *int start = 0, end = s.length() - 1;
      *while (s.charAt(start) == ' ') start++;
      *while (s.charAt(end) == ' ') end--;
      *return s.substring(start,end + 1);	substring(0,3)返回0到2之间的字符串
      */
        String[] danci = s.split(" ");
        /*	split()实现
      *
      *
      *
      *return 
      */
    	StringBuilder res = new StringBuilder();
        for (int i = danci.length - 1; i >= 0; i--){
            if (danci[i].equals("")) continue;	//基本类型char使用==判断是否一致，String使用equals()判断
            res.append(" " + danci[i]);
        }
        return res.toString().trim();
    }
```

###### *26.扑克牌中的顺子	HashSet判断重复

​	从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

```java
输入: [1,2,3,4,5]
输出: True
```

```java
输入: [0,0,1,2,5]
输出: True
```

```java
public static boolean isStraight(int[] nums) {
    /*满足两个条件：
      *1.除0以外不重复；
      *2.数组中max - min < 5
      */
        Arrays.sort(nums);
        int p = 0, q = nums.length-1;
        for (; nums[p] == 0; p++);	//	while(nums[p++]==0);	error
        for (int i = p; i < q; i++){
            if (nums[i] == nums[i+1])   return false;	//已排序的数组，相邻重复
        }
        if (nums[q]-nums[p] < 5)    return true;
        return false;
    }
```

###### <span style="color : red">*27.圆圈中最后剩下的数字		**约瑟夫环**</span>

0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

```
输入: n = 5, m = 3
输出: 3

输入: n = 10, m = 17
输出: 2
```

```java
public static int lastRemaining(int n, int m) {
    //链表实现
        List<Integer> list = new ArrayList<>();	//LinkedList超时
        for (int i = 0; i < n; i++)
            list.add(i);
        for (int idx = 0; n > 1; n--){	//idx：移除数的下标
            idx = (idx + m - 1) % n;	//删除节点下标的递推公式
            list.remove(idx);
        }
        return list.get(0);
    }
```

```java
public static int lastRemaining(int n, int m) {
    /*********数学实现********/
        int res = 0;	//res为最终数字的下标
        for (int i = 2; i <= n; i++){	//从n = 2开始反推
            res = (res + m) % i;	//（公式）由结果往前推导出 原数组中 最终数字的下标
        }
        return res;	//下标与数字相同
    }
```

```
res=(res+m)%n		  n				  数组后面加上数组的复制来体现*环状*
3=(0+3)%5					5				0 1 2 3 4 | 0 1 2 3 4
0=(1+3)%4					4				3 4 0 1 | 3 4 0 1
1=(1+3)%3					3				1 3 4 | 1 3 4
1=(0+3)%2					2				1 3 | 1 3
0	(由下往上)				1				3
```

###### <span style="color : red">*28.二叉搜索树的最近公共祖先</span>

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。

说明：
·所有节点的值都是唯一的。
·p、q 为不同节点且均存在于给定的二叉搜索树中。
```

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    //二叉搜索树左小右大
    //p,q的值均小于root，则最近公共祖先在root.left
    //root的值介于p,q之间，则返回root
        while (root != null){
            if (root.val < p.val && root.val < q.val){
                root = root.right;
            }else if (root.val > q.val && root.val > p.val){
                root = root.left;
            }else {
                break;
            }
        }
        return root;
    }
```

###### <span style="color : red">*29.二叉树的最近公共祖先</span>

```
BST推广到普通二叉树
```

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null){
            return null;
        }
        if (root == p || root == q){
            return root;
        }
    //后序遍历
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p ,q);
        if (left != null && right != null){	//p,q分别在左右子树中
            return root;
        }
        if (left != null){	//p,q在左子树中
            return left;
        }
        if (right != null){	//p,q在右子树中
            return right;
        }
        return null;
    }
```

