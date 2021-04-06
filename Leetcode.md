###### 	1.翻转二叉树/二叉树的镜像

```
输入：
 4
/   \
2     7
/ \   / \
1   3 6   9
输出：
 4
/   \
7     2
/ \   / \
9   6 3   1
```

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

###### 2.用两个栈实现一个队列

> ​	队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )
>

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

>  [1,   4,  7, 11, 15],			
>   [2,   5,  8, 12, 19],			
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],
>   [18, 21, 23, 26, 30]
> 每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。完成一个函数，输入一个二维数组和一个整数，判断数组中是否含有该整数。

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        boolean flag = false;
        int i = matrix.length - 1;
        int j = 0;
        //[matrix.length - 1,0]处于 行最大列最小的位置。[0,matrix[0].length - 1]处也可以
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

> 请实现有重复数字的升序数组的二分查找。
> 输出在数组中第一个大于等于查找值的位置，如果数组中不存在这样的数，则输出数组长度加一。
> 输入：5,4,[1,2,4,4,5]
> 输出：3

```java
public int upper_bound_ (int n, int v, int[] a) {	//n:数组a长度；v查找值
        if (v>a[n-1]) return n+1;
        int left = 0, right = n-1;
        int mid = (right-left)/2+1;
        while (left!=right){
            if (a[mid]>=v){
                right = mid;
            }else{
                left = mid+1;	//加一，否则死循环
            }
            mid = (right-left)/2+left;
        }
        return mid+1;
    }
```

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
public static ListNode reverseList(ListNode head) {
        if(head == null)return head;
        ListNode nhead = new ListNode(0);
        nhead.next = head;
        ListNode pre = head;
        ListNode cur = null;
    //空指针异常
        while(pre.next != null){
            cur = pre.next;
            pre.next = cur.next;
            cur.next = nhead.next;
            nhead.next = cur;
        }
        return nhead.next;
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

```
二叉树 [1,2,2,3,4,4,3] 是对称的。
 1
/ \
2  2
/ \ / \
3  4 4  3

[1,2,2,null,3,null,3] 则不是镜像对称的:
 1
/ \
2  2
\  	\
3   	3
```

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

> 输入：arr = [3,2,1], k = 2
> 输出：[1,2] 或者 [2,1]

```java
public int[] getLeastNumbers(int[] arr, int k) {
       int[] res = new int[k];
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(new Comparator<Integer>(){
            public int compare(Integer o1, Integer o2){
                return o1-o2;
            }
        });
        for (int i = 0; i < arr.length; i++) {
            priorityQueue.add(arr[i]);
        }
        for (int i = 0; i < k; i++) {
            res[i] = priorityQueue.poll();
        }
        return res;
    }
```

###### *17.连续子数组的最大和	<font color="#FF0000">**动态规划**</font>

> 输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

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

> 字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
> 输入s = "abaccdeff"
> 返回 "b"
> 输入s = "" 
> 返回 " "

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

###### <font color="#FF0000">*21.二叉树的最大深度</font>

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

> 输入：target = 15
> 输出：[[1,2,3,4,5],[4,5,6],[7,8]]

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

> 从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
> 输入: [1,2,3,4,5]
> 输出: True
> 输入: [0,0,1,2,5]
> 输出: True

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

> 0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
> 输入: n = 5, m = 3
> 输出: 3
> 输入: n = 10, m = 17
> 输出: 2

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

> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
> 输出: 6 
> 解释: 节点 2 和节点 8 的最近公共祖先是 6。
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
> 输出: 2
> 解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
>
> 说明：
> ·所有节点的值都是唯一的。
> ·p、q 为不同节点且均存在于给定的二叉搜索树中。

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

> BST推广到普通二叉树

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

###### <span style="color : red">*30.两数相加（不用加减乘除）</span>

```java
public static int add(int a, int b){
    while(b != 0){	//当进位不为0进入循环
        int n = a ^ b;	//本位==a异或b
        b = (a&b)>>1;	//进位==a与b**向左移位**
        a = n;	//本位与进位相加
    }
    return a;
}
```

###### <span style="color : red">*31.已知前序遍历和中序遍历，还原一棵二叉树</span>

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    if (preorder.length == 0)	return null;
    TreeNode root = new TreeNode(preorder[0]);
    int i = 0;
    for (; i < inorder.length; i++){
        if (root.val == inorder[i])	break;
    }
    int[] preleft = Arrays.copyOfRange(preorder,1,i+1);
    int[] inleft = Arrays.copyOfRange(inorder,0,i);
    int[] preright = Arrays.copyOfRange(preorder,i+1,preorder.length);
    int[] inright = Arrays.copyOfRange(inorder,i+1,inorder.length);
    root.left = buildTree(preleft, inleft);
    root.right = buildTree(preright, inright);
    return root;
}
```

###### <span style="color : red">*32.矩阵中的路径	dfs/回溯</span>

> 输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
> 输出：true
> 从任意一格开始，如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。

```java
public boolean exist(char[][] board, String word) {
        char[] words = word.toCharArray();
        if (board.length == 0) return false;
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (backTrack(i,j,0,board,words))   return true;
            }
        }
        return false;
    }
    public boolean backTrack(int i, int j, int k, char[][] board, char[] words){
        if (i<0||j<0||i>=board.length||j>=board[0].length||board[i][j]!=words[k]) return false;
        if (k == words.length-1)  return true;
        board[i][j] = '\0';	//做选择
        boolean res = backTrack(i+1,j,k+1,board,words)||backTrack(i-1,j,k+1,board,words)||
                backTrack(i,j+1,k+1,board,words)||backTrack(i,j-1,k+1,board,words);
        board[i][j] = words[k];	//撤销选择
        return res;
    }
```

###### <span style="color : red">*33.机器人运动范围	dfs/回溯	bfs</span>

> 地上有一个m行n列的方格，
>
> 从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？
> 输入：m = 2, n = 3, k = 1
> 输出：3

```java
public int movingCount(int m, int n, int k) {
        boolean[][] myArray = new boolean[m][n];
        return robotBackTrack(m,n,0,0,k,myArray);
    }
	//DFS
    public int robotBackTrack(int m, int n, int i, int j, int k,boolean[][] array){
        if (i<0||j<0||i>=m||j>=n||((mySum(i)+mySum(j))>k)||array[i][j]) return 0;
        array[i][j] = true;	//做选择
        return 1+robotBackTrack(m,n,i+1,j,k,array)+robotBackTrack(m,n,i,j+1,k,array);
    }
    public int mySum(int num){
        int sum = 0;
        while (num != 0){
            sum += num % 10;
            num = num / 10;
        }
        return sum;
    }
```

```java
public int movingCount(int m, int n, int k) {
        boolean[][] myArray = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        int count = 0;
    	//BFS
        queue.offer(new int[]{0,0});
        while (!queue.isEmpty()){
            int[] out = queue.poll();
            int i = out[0]; int j = out[1];
            if (i<0||j<0||i>=m||j>=n||mySum(i)+mySum(j)>k||myArray[i][j]) continue;	//排除掉不符合的情况 不累加
            myArray[i][j]=true;	//作标记
            count++;
            queue.offer(new int[]{i+1,j});	//向下
            queue.offer(new int[]{i,j+1});	//向右
        }
        return count;
    }
    public int mySum(int num){
        int sum = 0;
        while (num != 0){
            sum += num % 10;
            num = num / 10;
        }
        return sum;
    }
```



🐮客

###### 34.链表中是否有环？

```java
public boolean hasCycle(ListNode head) {
        if (head == null) return false;
        ListNode fast = head;
        ListNode slow = head;
        while(fast!= null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) return true;
        }
        return false;
    }//快慢指针
```

###### 34拓展.环的入口节点

> 对于一个给定的链表，返回环的入口节点，如果没有环，返回null

```java
public ListNode detectCycle(ListNode head) {
        if (hasCircle(head)){
            ListNode fast = head, slow = head;
            while (fast != null && fast.next != null){
                fast = fast.next.next;
                slow = slow.next;
                if (fast == slow) break;
            }
            slow = head;	//指回头节点，按相同速度前进
            while (fast != slow){
                fast = fast.next;
                slow = slow.next;
            }
            return fast;
        }
        return null;
    }
    public boolean hasCircle(ListNode head) {
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) return true;
        }
        return false;
    }
```

###### 35.链表中的节点每k个一组翻转

> 将给出的链表中的节点每  k 个一组翻转，返回翻转后的链表
> 如果链表中的节点数不是 k 的倍数，将最后剩下的节点保持原样

```java
public ListNode reverseKGroup (ListNode head, int k) {
        if (head == null || head.next == null || k == 1) return head;
        ListNode nhead = new ListNode(0);
        nhead.next = head;
        ListNode pre = head, cur = null, p = nhead;
        int length = 0;
        while (head != null){
            head = head.next;
            length++;
        }
        for (int i = 0; i < length/k; i++) {
            for (int j = 1; j < k; j++) {	//每K个一组翻转，操作K-1次
                cur = pre.next;
                pre.next = cur.next;
                cur.next = p.next;
                p.next = cur;
            }
            p = pre;
            pre = pre.next;
        }
        return nhead.next;
    }
```

###### 36.LRU缓存设计

> 设计LRU缓存结构，该结构在构造时确定大小，假设大小为K，并有如下两个功能
> set(key, value)：将记录(key, value)插入该结构
> get(key)：返回key对应的value值
> [要求]
> 	set和get方法的时间复杂度为O(1)
> 	某个key的set或get操作一旦发生，认为这个key的记录成了最常使用的。
> 	当缓存的大小超过K时，移除最不经常使用的记录，即set或get最久远的。
> 若opt=1，接下来两个整数x, y，表示set(x, y)
> 若opt=2，接下来一个整数x，表示get(x)，若x未出现过或已被移除，则返回-1
> 对于每个操作2，输出一个答案

```java
class LRU {
    //通过 HashMap+双向链表 实现
    private class DLinkedNode {
        int key;
        int value;
        DLinkedNode pre;
        DLinkedNode next;
        public DLinkedNode(){}
        public DLinkedNode(int key, int value){
            this.key = key;
            this.value = value;
        }
    }
   private HashMap<Integer,DLinkedNode> map = new HashMap<>();
   private DLinkedNode head = new DLinkedNode(-1,-1);
   private DLinkedNode tail = new DLinkedNode(-1,-1);
   private int k;
    public int[] LRU(int[][] operators, int k) {
        this.k = k;
        head.next = tail;
        tail.pre = head;
        int len = (int)Arrays.stream(operators).filter(x -> x[0] == 2).count();
        //stream操作
        int[] res = new int[len];
       for (int i = 0, j = 0; i < operators.length; i++){
            if (operators[i][0]==1){
                put(operators[i][1],operators[i][2]);
            }else {
                res[j++] = get(operators[i][1]);
            }
        }
        return res;
    }
    
    public int get(int key) {
        if (map.containsKey(key)){
            DLinkedNode node = map.get(key);
            node.pre.next = node.next;
            node.next.pre = node.pre;
            //放到头节点之后
            node.next = head.next;
            head.next.pre = node;
            head.next = node;
            node.pre = head;
            return node.value;
        }else {
            return -1;
        }
    }
    public void put(int key, int value) {
        if (map.containsKey(key)){
            map.get(key).value = value;
            DLinkedNode node = map.get(key);
            node.pre.next = node.next;
            node.next.pre = node.pre;
            //放到头节点之后
            node.next = head.next;
            head.next.pre = node;
            head.next = node;
            node.pre = head;
        }else {
            if (map.size() == k){
                int tempkey = tail.pre.key;
                tail.pre.pre.next = tail;
                tail.pre = tail.pre.pre;
                map.remove(tempkey);
            }
            DLinkedNode node = new DLinkedNode(key,value);
            node.next = head.next;
            head.next.pre = node;
            head.next = node;
            node.pre = head;
            map.put(key,node);
        }
    }
}
```

###### 37.二叉树的先序、中序，后序遍历

> 输出成一个二维数组

```java
List<Integer> prelist = new ArrayList<>();
List<Integer> inlist = new ArrayList<>();
List<Integer> afterlist = new ArrayList<>();
public int[][] threeOrders (TreeNode root) {
    int[][] array = {preOrder(root),
                     inOrder(root),
                     afterOrder(root)};
    return array;
}
public int[] preOrder (TreeNode root) {
    if (root == null) return null;
    prelist.add(root.val);
    preOrder(root.left);
    preOrder(root.right);
    int[] res = new int[prelist.size()];
    for (int i = 0; i < prelist.size(); i++) {
        res[i] = prelist.get(i);
    }
    return res;
}
public int[] inOrder (TreeNode root) {
    if (root == null) return null;
    inOrder(root.left);
    inlist.add(root.val);
    inOrder(root.right);
    int[] res = new int[inlist.size()];
    for (int i = 0; i < inlist.size(); i++) {
        res[i] = inlist.get(i);
    }
    return res;
}
public int[] afterOrder (TreeNode root) {
    if (root == null) return null;
    afterOrder(root.left);
    afterOrder(root.right);
    afterlist.add(root.val);
    int[] res = new int[afterlist.size()];
    for (int i = 0; i < afterlist.size(); i++) {
        res[i] = afterlist.get(i);
    }
    return res;
}
```

###### 38.据二叉树的前序遍历，中序遍历恢复二叉树，并打印出二叉树的右视图

```java
public int[] solve (int[] xianxu, int[] zhongxu) {
        return rightSeen(back(xianxu,zhongxu));
    }
//恢复二叉树
    public TreeNode back (int[] pre, int[] in){
        if (pre.length==0||in.length==0) return null;
        TreeNode root = new TreeNode(pre[0]);
        int i;
        for (i = 0; i < in.length; i++){
            if (in[i]==pre[0]) break;
        }
        int[] leftpre = Arrays.copyOfRange(pre,1,i+1);
        int[] rightpre = Arrays.copyOfRange(pre,i+1,pre.length);
        int[] leftin = Arrays.copyOfRange(in,0,i);
        int[] rightin = Arrays.copyOfRange(in,i+1,in.length);
        root.left = back(leftpre,leftin);
        root.right = back(rightpre,rightin);
        return root;
    }
//输出右视图
    public int[] rightSeen (TreeNode root){
        if (root==null) return null;
        Queue<TreeNode> queue = new LinkedList<>();
        List<Integer> res = new ArrayList<>();
        queue.offer(root);
        while (!queue.isEmpty()){
            TreeNode temp = root;
            for (int i = queue.size(); i > 0; i--){
                temp = queue.poll();
                if (temp.left!=null) queue.offer(temp.left);
                if (temp.right!=null) queue.offer(temp.right);
            }
            res.add(temp.val);
        }
        return res.stream().mapToInt(Integer::intValue).toArray();	//list to array
    }
```

###### 39.二叉树 之 字搜索

```java
public ArrayList<ArrayList<Integer>> zigzagLevelOrder (TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if (root==null)    return res;
        int floor = 1;
        queue.offer(root);
        while (!queue.isEmpty()){
            ArrayList<Integer> ceng = new ArrayList<>();
            for (int size = queue.size(); size > 0; size--){
                TreeNode temp = queue.poll();
                //将弹出节点的左右节点入队，而不是root左右节点。
                ceng.add(temp.val);
                if (temp.left!=null) queue.offer(temp.left);
                if (temp.right!=null) queue.offer(temp.right);
            }
            //正常层序遍历后翻转输出
            if (floor%2==0) Collections.reverse(ceng);
            res.add(ceng);
            floor++;
        }
        return res;
    }
```

###### 40.数组中最长无重复子数组的长度 (连续)

```java
public int maxLength (int[] arr) {
        HashMap<Integer,Integer> map = new HashMap<>();
        int left = 0,right = 0,maxLength = 0;
        for (; right < arr.length; right++){
            if (map.containsKey(arr[right])){
                left = Math.max(left,map.get(arr[right])+1);	//[1,3,4,3,4]	left指向重复数的下一个，保证left，right之间无重复元素
            }
            maxLength = Math.max(maxLength,right-left+1);	//新一轮子数组长度 大于 上一轮，取最大
            map.put(arr[right],right);
        }
        return maxLength;
    }
```

###### 41.合并K个已排序链表

> 输入：[{1,2,3},{4,5,6,7}]
> 输出：{1,2,3,4,5,6,7}

```java
//归并思想 + 递归
public ListNode mergeKLists(ArrayList<ListNode> lists) {
        if (lists.size()<=0) return null;
        ArrayList<ListNode> temp = new ArrayList<>();
        if (lists.size()==1) return lists.get(0);
        if (lists.size()%2!=0) lists.add(null);	//如果list大小为奇数，补成偶数
        for (int i = 0; i < lists.size(); i=i+2){
            temp.add(mergeTwoLists(lists.get(i),lists.get(i+1)));
        }
        return mergeKLists(temp);
    }
//基于	合并两个已排序链表
    public ListNode mergeTwoLists(ListNode l1, ListNode l2){
        ListNode head = new ListNode(-1);
        ListNode p = head;
        while ((l1!=null)&&(l2!=null)){
            if (l1.val>l2.val){
                p.next = l2;
                l2 = l2.next;
            }else {
                p.next = l1;
                l1 = l1.next;
            }
            p = p.next;
        }
        if (l1==null) p.next = l2;
        else p.next = l1;
        return head.next;
    }
```

###### 42.两个链表生成相加链表

```java
//翻转链表后相加
public ListNode addInList (ListNode head1, ListNode head2) {
    ListNode nhead1 = reverseList(head1);
    ListNode nhead2 = reverseList(head2);
    ListNode nhead = new ListNode(-1);
    ListNode p = nhead;
    int carry = 0;	//进位
    //相加
    while (nhead1!=null||nhead2!=null){
        int standard = carry;	//每次 本位 设置等于进位
        if (nhead1!=null){
            standard += nhead1.val;
            nhead1 = nhead1.next;
        }
        if (nhead2!=null){
            standard += nhead2.val;
            nhead2 = nhead2.next;
        }
        p.next = new ListNode(standard%10);
        p = p.next;
        carry = standard/10;
    }
    if (carry>0){
        p.next = new ListNode(carry);
    }
    return reverseList(nhead.next);
}
public ListNode reverseList(ListNode head){
    ListNode nhead = new ListNode(-1);
    ListNode cur = null, pre = head;
    nhead.next = head;
    while (pre.next!=null){
        cur = pre.next;
        pre.next = cur.next;
        cur.next = nhead.next;
        nhead.next = cur;
    }
        return nhead.next;
    }
```

###### 43.删除链表的倒数第n个节点

```java
//翻转链表后删除
public ListNode removeNthFromEnd (ListNode head, int n) {
    if (n==0) return head;
    if (head.next==null) return null;
    head = reverseList(head);
    ListNode p = head;
    if (n==1) return reverseList(head.next);
    for (int i = 1;i < n-1; i++){
        p = p.next;
    }
    p.next = p.next.next;
    return reverseList(head);
}
public ListNode reverseList(ListNode head){
    ListNode nhead = new ListNode(-1);
    ListNode cur = null, pre = head;
    nhead.next = head;
    while (pre.next!=null){
        cur = pre.next;
        pre.next = cur.next;
        cur.next = nhead.next;
        nhead.next = cur;
    }
    return nhead.next;
}
```

###### 44.两个字符串的最长公共子串（连续）

```java
public String LCS (String str1, String str2) {
    int[][] dp = new int[str1.length()][str2.length()];
    //二维数组，str1为行，str2为列
    int maxlength = 0, tail = 0;
    for (int i = 0; i < str1.length(); i++){
        for (int j = 0; j < str2.length(); j++){
            if (str1.charAt(i)==str2.charAt(j)){
                if (i==0||j==0) dp[i][j] = 1;
                else{
                    dp[i][j] = dp[i-1][j-1]+1;
                }
                if (dp[i][j]>maxlength){
                    maxlength = dp[i][j];
                    tail = i;
                }
            }else{
                dp[i][j] = 0;
            }
        }
    }
    return str1.substring(tail-maxlength+1,tail+1);	//左闭右开
}
```

###### 	45.两个字符串的最长公共子序列（不连续）

> 给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长公共子序列的长度。
> 输入：text1 = "abcde", text2 = "ace" 
> 输出：3
> 解释：最长公共子序列是 "ace"，它的长度为 3。

```java
public int longestCommonSubsequence(String text1, String text2) {
        int length1 = text1.length()+1, length2 = text2.length()+1;
        int[][] dp = new int[length1][length2];
        for(int i = 0; i < len1+1; i++){
            for(int j = 0; j < len2+1; j++){
                //初始化dp[][]行列第一个元素
                if(i == 0 || j == 0){
                    dp[i][j] = 0;
                    continue;
                }
                if(s1.charAt(i-1) == s2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1]+1;
                }else{
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
//dp数组右下角的值就是累加的最大长度
        return dp[length1-1][length2-1];
    }
```

> 输出子序列

```java
public String LCS (String s1, String s2) {
    int length1 = s1.length();
    int length2 = s2.length();
    int[][] dp = new int[length1+1][length2+1];
    for (int i = 0;i < length1+1; i++){
        dp[i][0] = 0;
    }
    for (int j = 0;j < length1+1; j++){
        dp[0][j] = 0;
    }
    for (int i = 1; i < length1+1; i++){
        for (int j = 1; j < length2+1; j++){
            if (s1.charAt(i-1)==s2.charAt(j-1)){
                dp[i][j] = dp[i-1][j-1]+1;
            }else{
                dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
            }
        }
    }
//通过dp[][]数组还原子序列
    StringBuilder sb = new StringBuilder();
    int len1 = length1-1,len2 = length2-1;
    while (len1>=0&&len2>=0){
        if (s1.charAt(len1)==s2.charAt(len2)){
            sb.append(s1.charAt(len1));
            len1--;
            len2--;
        }else{
            if (dp[len1][len2+1]>dp[len1+1][len2]) len1--;
            else len2--;
        }
    }
    if (sb.length()==0) return "-1";
    return sb.reverse().toString();
}
```



###### 46.括号序列

> 给出一个仅包含字符'(',')','{','}','['和']',的字符串，判断给出的字符串是否是合法的括号序列。括号必须以正确的顺序关闭，"()"和"()[]{}"都是合法的括号序列，但"(]"和"([)]"不合法。

```java
public boolean isValid (String s) {
    Stack<Character> stack = new Stack<>();
    char[] array = s.toCharArray();
    for (int i = 0; i < array.length; i++){
        if (array[0]==')'||array[0]=='}'||array[0]==']') return false;
        if (array[i]=='('||array[i]=='{'||array[i]=='['){
            stack.push(array[i]);
        }else if (!stack.isEmpty()){
            if (array[i]==')'&&stack.pop()=='(') continue;
            if (array[i]=='}'&&stack.pop()=='{') continue;
            if (array[i]==']'&&stack.pop()=='[') continue;
        }else return false;
    }
    if (stack.isEmpty()) return true;
    return false;
}
```

###### 47.字符串的**全排列**	回溯

> 输入: "ab"
> 输出: ["ab","ba"]

```java
public class Solution {
    private ArrayList<String> res = new ArrayList<>();
    private StringBuilder temp = new StringBuilder();
    private TreeSet<String> treeset = new TreeSet<>();
    private boolean[] visited;

    public ArrayList<String> Permutation(String str) {
        if (str == null || str.equals("")) {
            return res;
        }
        char[] strs = str.toCharArray();
        visited = new boolean[strs.length];
        quanpailie(0,strs);
        res.addAll(treeset);
        return res;
    }
    public void quanpailie (int index, char[] strs){
        if (index==strs.length){
            treeset.add(temp.toString());
            return;
        }
        for (int i = 0; i < strs.length; i++){
            if (!visited[i]){
                visited[i] = true;
                temp.append(strs[i]);
                quanpailie(index+1,strs);
                visited[i] = false;
                temp.deleteCharAt(temp.length()-1);
            }
        }
    }
}
```

###### 48.判断二叉树和为指定值的路径是否存在 dfs

```java
boolean flag = false;
    public boolean hasPathSum (TreeNode root, int sum) {
        hasPath(root,sum,0);
        return flag;
    }
    public void hasPath (TreeNode root, int sum, int cnt) {
        if (root==null||flag==true) return;
        cnt += root.val;
        if (cnt==sum&&root.left==null&&root.right==null){
            flag = true;
        }
        //前序遍历
        hasPath(root.left,sum,cnt);
        hasPath(root.right,sum,cnt);
    }
```

###### 49.输出二叉树和为指定值的路径的集合	dfs/回溯

> 输入：{1,2},3
> 输出：[[1,2]]

```java
ArrayList<ArrayList<Integer>> res = new ArrayList<>();
ArrayList<Integer> path = new ArrayList<>();
public ArrayList<ArrayList<Integer>> pathSum (TreeNode root, int sum) {
    backTrace(root,sum,0);
    return res;
}
public void backTrace(TreeNode root, int sum, int cnt){
    if (root==null) return;
    path.add(root.val);
    cnt += root.val;
    if (root.left==null&&root.right==null&&cnt==sum)
        //*path是全局对象，会被覆盖，直接传path仅会保留最后一个，故每次通过构造函数new一个新ArrayList
        res.add(new ArrayList<>(path));	
    //N叉树的遍历无法用 回溯模板 中的for循环，直接顺序列出即可
    backTrace(root.left,sum,cnt);
    backTrace(root.right,sum,cnt);

    list.remove(list.size()-1);	//移除最后一个
}
```

###### 50.岛屿数量问题 dfs

> 1代表是陆地，0代表海洋， 如果两个1相邻，那么这两个1属于同一个岛。相邻陆地可以组成一个岛屿（相邻:上下左右）
> 输入：[[1,1,0,0,0],[0,1,0,1,1],[0,0,0,1,1],[0,0,0,0,0],[0,0,1,1,1]]
> 输出：3

```java
public int solve (char[][] grid) {
        int row = grid.length;
        int col = grid[0].length;
        int res = 0;
        for (int i = 0; i < row; i++){
            for (int j = 0; j < col; j++){
                //遍历岛屿左上角
                if (grid[i][j]=='1'){
                    res++;
                    dfs(grid,i,j);
                }
            }
        }
        return res;
    }
    public void dfs(char[][] grid, int i, int j) {
        if (grid.length==0) return;
        if (i<0||j<0||i>grid.length-1||j>grid[0].length-1||grid[i][j]=='0') return;
        grid[i][j] = '0';	//从左上角把'1'均置为'0'
        dfs(grid,i+1,j);
        dfs(grid,i-1,j);
        dfs(grid,i,j+1);
        dfs(grid,i,j-1);
    }
```

###### 51.最长回文子串

```java
//翻转求两个字符串的最长公共子串 好像行不通
public int getLongestPalindrome(String A, int n) {
    int maxlength = 0;
    for (int i = 0; i < n; i++){
        for (int j = i+1; j <= n; j++){	//暴力遍历
            if (isPalindrome(A.substring(i,j))&&(j-i)>maxlength)
                maxlength = j-i;
        }
    }
    return maxlength;
}
public boolean isPalindrome(String s) {
    char[] array = s.toCharArray();
    int left = 0, right = array.length-1;
    while (left<right){
        if (array[left]!=array[right])
            return false;
        left++;
        right--;
    }
    return true;
}
```

###### 52.重排链表	&&	判断回文链表

> 对于给定的单链表{10,20,30,40}，将其重新排序为{10,40,20,30}.

```java
//***************重排****************
public void reorderList(ListNode head) {
    if (head==null||head.next==null||head.next.next == null) return;
    ListNode p = head;
    ListNode mid = getMiddle(head);
    ListNode q = mid.next;
    mid.next = null;	//中间左边为尾节点，next指向null
    q = reverse(q);	//获取中间节点后，后面部分链表反转，然后顺序遍历
    while (q!=null){
        ListNode tmp = q.next;
        q.next = p.next;
        p.next = q;
        p = q.next;
        q = tmp;
    }
}
//***************回文***************
public boolean isPail (ListNode head) {
        ListNode mid = getMiddle(head).next;
        mid = reverse(mid);
        while (mid!=null){
            if (mid.val!=head.val) return false;
            mid = mid.next;
            head = head.next;
        }
        return true;
    }
//获取中间节点：奇数是中间节点，偶数是中间左边节点
public ListNode getMiddle(ListNode head){
    if(head==null) return null;
    ListNode fast = head.next;
    ListNode slow = head;
    while(fast!=null&&fast.next!=null){
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
}
public ListNode reverse(ListNode head){
    ListNode nhead = new ListNode(0);
    nhead.next = head;
    ListNode pre = head;
    ListNode cur = null;
    while(pre.next!=null){
        cur = pre.next;
        pre.next = cur.next;
        cur.next = nhead.next;
        nhead.next = cur;
    }
    return nhead.next;
}
```

###### 53.一次股票

> 假设你有一个数组，其中第*i*个元素是股票在第*i*天的价格。
> 你有一次买入和卖出的机会。（只有买入了股票以后才能卖出）。请你设计一个算法来计算可以获得的最大收益。
> 输入：[1,4,2]
> 返回：3

```java
//滑动窗口
public int maxProfit (int[] prices) {
    int left = 0, right = 0;
    int length = prices.length;
    int[] temp = new int[length];
    temp[0] = 0;
    int max = 0, sum = 0;
    for (int i = 1; i < length; i++) {
        temp[i] = prices[i] - prices[i - 1];
    }
    while (right<length){
        //**累加和为负数**才改变头指针位置，累加和为正说明之前的变化对总和的贡献是正的
        //而不是一碰到负的情况就改变头指针位置
        if (sum(temp,left,right)<0){
            left = right;
        }else {
            if (sum(temp,left,right)>max)
                max = sum(temp,left,right);
        }
        right++;
    }
    return max;
}
public int sum(int[] arr,int left, int right){
    int sum = 0;
    for (int i = left+1; i <= right; i++) {
        sum += arr[i];
    }
    return sum;
}
```





#### 排序

```java
//快排 递归+分治
public int[] MySort (int[] arr) {
    fastSort(arr,0,arr.length-1);
    return arr;
}
public void fastSort (int[] array,int start, int length) {
    if (start>=length)  return;
    int p = exchange(array,start,length);	//返回array[0]的新位置
    fastSort(array,start,p-1);	//对p左边部分进行同样操作
    fastSort(array,p+1,length);
}
public int exchange(int[] array, int start, int last) {
    int temp = array[start];	//取array[0]为比较基准，小于temp的放左边，大于的放右边。
    //头尾双指针
    int i = start;
    int j = last+1;
    while (true) {
        while (array[++i]<temp){
            if (i==last) break;
        }
        while (array[--j]>temp){
            if (j==start) break;
        }
        if (i>=j) break;
        swap(array,i,j);
    }
    swap(array,start,j);
    return j;	//为什么返回j，不是i
}
public void swap(int[] array, int i, int j){
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
}
```

##### 利用快排找出数组中第K大的数

```java
public int findKth(int[] a, int n, int K) {
        if (K>n) return -1;
        fastSort(a,0,n-1,K);
        return a[K-1];
    }
    public void fastSort (int[] array, int start, int last, int K){
        if (start>last) return;
        int p = exchange(array,start,last);
        if (p>K-1){	//剪枝
            fastSort(array,0,p-1,K);
        }else {
            fastSort(array,p+1,last,K);
        }
    }
    public int exchange(int[] array, int start, int last){
        int temp = array[start];
        int i = start;
        int j = last+1;
        while (true){
            while(array[++i]>temp){
                if (i>=last) break;
            }
            while(array[--j]<temp){
                if (j<=start) break;
            }
            if (i>=j) break;
            swap(array,i,j);
        }
        swap(array,start,j);
        return j;
    }
    public void swap(int[] array, int i, int j){
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
```



```java
//堆排

```



```java
//16题shell排序
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

#### 集合遍历

```java
//*************HashSet**************
HashSet<Integer> set = new HashSet<>();
Iterator<Integer> it = set.iterator();
while (it.hasNext()){
    Integer tmp = it.next();
    if (...) {
        it.remove();	//遍历时移除元素
    }
    ...
}
//************************************
//*************HashMap**************
//entrySet (key && value)
HashMap<Integer,Integer> map = new HashMap<>();
Set<Map.Entry<Integer, Integer>> entrySet = map.entrySet();
Iterator<Map.Entry<Integer, Integer>> it = entrySet.iterator();
while (it.hasNext){
    it.next();
    ...
}
//KeySet (only key)
HashMap<Integer,Integer> map = new HashMap<>();
Set<Integer> set = map.KeySet();
Iterator<Integer> it = set.iterator();
while (it.hasNext){
    it.next();
    ...
}
//************************************
//*************ArrayList**************
for(int i = 0; i < list.size(); i++){
    list.get(i);
    ...
}
```

#### 字符串操作

```java

```

#### 排序

```java
ArrayList<Integer> list = new ArrayList<>();

Collections.sort(list);	//直接传
Collections.sort(list, new Comparator<Integer>(){	//Comparator自定义比较
    public int compare(Integer o1, Integer o2) {
                    return o1-o2;
    }
});//等价于：
Collections.sort(list, (o1,o2)->o1-o2);

list.sort(new Comparator<Integer>(){	//同样可以自定义
    public int compare(Integer o1, Integer o2) {
                    return o1-o2;
    }
});//等价于：
list.sort((o1,o2)->o1-o2);
```

