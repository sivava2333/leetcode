# 1. 树状数组的作用
+ 解决数组元素更新后，前缀和数组更新问题

# 2. index tree的结构
+ 元素个数相等就“合”一起

**Example: **
```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]

 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16
    1     3     5     7      9      11      13      15
          2           6             10              14
          1           5              9              13
                      4                             12
                      3                             11
                      2                             10
                      1                              9
                                                     8
                                                     7
                                                     6
                                                     5
                                                     4
                                                     3
                                                     2
                                                     1
```

# 3. 规律
1. 快速确定位置i是哪些位置的和：  
> 假设index的二进制是`010111000`,它包含的数是从去掉末尾的1再或上1，到它本身。即：`010110001`到`010111000`
2. 快速得到1~i位置的前缀和
> help[i] + while (i > 0) {help[i去掉末尾1]}

# 4. 代码
```
public class IndexTree {
    private int[] tree;
    private int N;
    
    public IndexTree(int size) {
        N = size;
        // 0位置弃用，下标从1开始
        tree = new int[N + 1];
    }
    
    public int sum(int index) {
        int ret = 0;
        while (index > 0) {
            ret += tree[index];
            // index & -index 得到最末尾的1
            index -= index & -index;
        }
        
        return ret;
    }
    
    // 加最右侧的1
    public void add(int index, int d) {
        while (index <= N) {
            tree[index] += d;
            index += index & -index;
        }
    }
}
```

# 5. 特点
+ 优点：
1. 代码简单易实现
2. 能够很好的处理基于二维数组的问题

+ 缺点：
1. 如果是对数组多个元素同时更新，效率要逊于线段树
