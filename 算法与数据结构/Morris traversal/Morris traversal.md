# 0. 算法作用
优化二叉树的遍历：可以在O(N)的时间复杂度，以及O(1)的空间复杂度完成二叉树的遍历。  
morris遍历可以完成二叉树的前中后序遍历
# 1. 算法主要流程
当前节点cur，一开始在整棵树的根节点

1. cur无左树，向右走，cur = cur.right
2. cur有左树，找到左树最右节点，将其右指针指向自己，然后向左走
    1. `如果mostright的右指针指向null`  
    **mostright.right = cur, cur = cur.left**  
    2. `mostright的右指针指向cur`  
    **mostright.right = null, cur = cur.right**

**Example 1 :**
```
Input:
     1
   /   \
  2     3
 / \   / \
4   5 6   7

Output:
1, 2, 4, 2, 5, 1, 3, 6, 3, 7
该结果为morris序：有左树的节点访问两次

只在第一次访问的时候打印，则为前序遍历：
1, 2, 4, 5, 3, 6, 7

有左树的节点，第二次访问的时候打印；无左树的节点第一次访问的时候打印，则为中序遍历：
4, 2, 5, 1, 6, 3, 7
```


# 2. 代码
```
public class MorrisTraversal {
    public class Node {
        public int value;
        Node left;
        Node right;
        
        public Node(int data) {
            this.value = data;
        }
    }
    
    public void morris(Node head) {
        if (head == null) {
            return ;
        }
        Node cur = head;
        Node mostRight = null;
        while (cur != null) {
            mostRight = cur.left;
            if (mostRight != null) {
                while (mostRight.right != null && mostRight.right != cur) {
                    mostRight = mostRight.right;
                }
                if (mostRight.right == null) {
                    mostRight.right = cur;
                    // 先序遍历操作，part1
                    // 有左子树的节点，第一次访问在这里
                    System.out.print(cur.value + " ");
                    cur = cur.left;
                    continue;
                } else {
                    mostRight.right = null;
                }
            }
            // 先序遍历操作，part2
            // 无左子树的节点，第一次访问在这里
            else {
                System.out.print(cur.value + " ");
            }
            // 中序遍历在这里操作.
            // 这里是有左子树的节点的第二次访问，无左子树节点的第一次访问
            System.out.print(cur.value + " ");
            cur = cur.right;
        }
    }
}
```
+ morris遍历实现后序遍历  
打印的时机在能访问2次，且第二次访问的时候，逆序打印左树的右边界。最后逆序打印整棵树的右边界
```
public class MorrisTraversal {
    public class Node {
        public int value;
        Node left;
        Node right;
        
        public Node(int data) {
            this.value = data;
        }
    }
    
    public void morris(Node head) {
        if (head == null) {
            return ;
        }
        Node cur = head;
        Node mostRight = null;
        while (cur != null) {
            mostRight = cur.left;
            if (mostRight != null) {
                while (mostRight.right != null && mostRight.right != cur) {
                    mostRight = mostRight.right;
                }
                if (mostRight.right == null) {
                    mostRight.right = cur;
                    cur = cur.left;
                    continue;
                } else {
                    mostRight.right = null;
                    printEdge(cur.left);
                }
            }
            cur = cur.right;
        }
        printEdge(head);
        System.out.println();
    }
    
    private void printEdge(Node head) {
        Node tail = reverseEdge(head);
        Node cur = tail;
        while (cur != null) {
            System.our.print(cur.value + " ");
            cur = cur.right;
        }
        
        reverseEdge(tail);
    }
    
    private Node reverseEdge(Node head) {
        Node pre = null;
        Node next = head;
        while (next != null) {
            Node tmp = next.right;
            next.right = pre;
            pre = next;
            next = tmp;
        }
        
        return pre;
    }
}
```

# 3. 练习
## 习题1
判断一棵树，是否是搜索二叉树

### 思路
中序遍历看结果是否是递增的  
中序遍历打印的位置判断节点的值是否是递增的

### 代码
```
public class MorrisTraversal {
    public class Node {
        public int value;
        Node left;
        Node right;
        
        public Node(int data) {
            this.value = data;
        }
    }
    
    public boolean isBST(Node head) {
        if (head == null) {
            return true;
        }
        Node cur = head;
        Node mostRight = null;
        Integer pre = null;
        while (cur != null) {
            mostRight = cur.left;
            if (mostRight != null) {
                while (mostRight.right != null && mostRight.right != cur) {
                    mostRight = mostRight.right;
                }
                if (mostRight.right == null) {
                    mostRight.right = cur;
                    cur = cur.left;
                    continue;
                } else {
                    mostRight.right = null;
                }
            }
            if (pre != null && pre >= cur.value) {
                return false;
            }
            cur = cur.right;
        }
        return true;
    }
}
```

## 习题2
二叉树的最小高度

### 思路
需要求出：
+ 当前节点的高度
+ 当前节点是否是叶子节点

### 代码
```
public class MorrisTraversal {
    public class Node {
        public int value;
        Node left;
        Node right;
        
        public Node(int data) {
            this.value = data;
        }
    }
    
    public int morris(Node head) {
        if (head == null) {
            return 0;
        }
        Node cur = head;
        Node mostRight = null;
        int curLevel = 0;
        int minHeight = Integer.MAX_VALUE;
        while (cur != null) {
            mostRight = cur.left;
            if (mostRight != null) {
                int leftHeight = 1;
                while (mostRight.right != null && mostRight.right != cur) {
                    leftHeight++;
                    mostRight = mostRight.right;
                }
                if (mostRight.right == null) {
                    curLevel++;
                    mostRight.right = cur;
                    cur = cur.left;
                    continue;
                } else {
                    if (mostRight.left == null) {
                        minHeight = Math.min(minHeight, curLevel);
                    }
                    curLevel -= leftHeight;
                    mostRight.right = null;
                }
            } else {
                curLevel++;
            }
            cur = cur.right;
        }
        // 处理最右节点
        int finalRight = 1;
        cur = head;
        while (cur.right != null) {
            finalRight++;
            cur = cur.right;
        }
        
        if (cur.left == null && cur.right == null) {
            minHeight = Math.min(minHeight, finalRight);
        }
        
        return minHeight;
    }
}
```
