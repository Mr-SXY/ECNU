###### 1.翻转二叉树

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

###### 3.找出数组中重复的数字。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {	//将数组元素添加进hashset
                repeat = num;
                break;
            }
        }
        return repeat;
    }
}
```

###### <font color="#FF0000">4.二维数组中的查找</font>

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
            else{
                flag = true;
            }
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
        for(char c : s.toCharArray()){
            if(c == ' '){	//''内表示char类型，""内表示String类型！！
                ns.append("%20");
            }else{
                ns.append(c);
            }
        }
        return ns.toString();
    }
}
```

###### 6.斐波那契数列	&&	青蛙跳台阶问题（一次跳1阶或2阶）

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

