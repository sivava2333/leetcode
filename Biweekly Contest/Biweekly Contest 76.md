# 0. Participant
2022/04/16 23:30:00   
rank: 2300

# 1. 2239. Find Closest Number to Zero -- easy
Given an integer array `nums` of size n, return the number with the value ***closest*** to `0` in `nums`. If there are multiple answers, return the number with the ***largest*** value.

+ Example 
```
Input: nums = [-4,-2,1,4,8]
Output: 1
Explanation:
The distance from -4 to 0 is |-4| = 4.
The distance from -2 to 0 is |-2| = 2.
The distance from 1 to 0 is |1| = 1.
The distance from 4 to 0 is |4| = 4.
The distance from 8 to 0 is |8| = 8.
Thus, the closest number to 0 in the array is 1.
```

+ Constraints:
 > 1 <= n <= 1000  
 > -105 <= nums[i] <= 105

## 1.1 Code
```
class Solution {
    public int findClosestNumber(int[] nums) {
        int ans = Integer.MAX_VALUE;
        for (int i : nums) {
            if (Math.abs(i - 0) < Math.abs(ans)) {
                ans = i;
            } else if (Math.abs(i - 0) == Math.abs(ans)) {
                ans = Math.max(ans, i);
            }
        }
        
        return ans;
    }
}
```

# 2. 2240. Number of Ways to Buy Pens and Pencils
You are given an integer `total` indicating the amount of money you have. You are also given two integers `cost1` and `cost2` indicating the price of a pen and pencil respectively. You can spend **part or all** of your money to buy multiple quantities (or none) of each kind of writing utensil.

Return the ***number of distinct*** ways you can buy some number of pens and pencils.

+ Example 1:
```
Input: total = 20, cost1 = 10, cost2 = 5
Output: 9
Explanation: The price of a pen is 10 and the price of a pencil is 5.
- If you buy 0 pens, you can buy 0, 1, 2, 3, or 4 pencils.
- If you buy 1 pen, you can buy 0, 1, or 2 pencils.
- If you buy 2 pens, you cannot buy any pencils.
The total number of ways to buy pens and pencils is 5 + 3 + 1 = 9.
```

+ Example 2:
```
Input: total = 5, cost1 = 10, cost2 = 10
Output: 1
Explanation: The price of both pens and pencils are 10, which cost more than total, so you cannot buy any writing utensils. Therefore, there is only 1 way: buy 0 pens and 0 pencils.
```

+ Constraints:
> 1 <= total, cost1, cost2 <= 106

## 2.2 Code
```
class Solution {
    public long waysToBuyPensPencils(int total, int cost1, int cost2) {
        long ans = 0;
        for (int i = 0; i * cost1 <= total; i++) {
            ans += (total - i * cost1) / cost2 + 1;
        }
        
        return ans;
    }
}
```

# 3. 2241. Design an ATM Machine
![image](https://user-images.githubusercontent.com/101119184/163684156-dcdf7341-727a-493a-88d1-81e7501e0222.png)
![image](https://user-images.githubusercontent.com/101119184/163684165-65245611-bec1-4476-942a-ed62474b3e00.png)

## 3.2 Code
```
class ATM {
    private int[] dict = new int[] {20, 50, 100, 200, 500};
    private long[] banknotes;
    //private long amount;
    
    public ATM() {
        banknotes = new long[5];
        //amount = 0;
    }
    
    public void deposit(int[] banknotesCount) {
        for (int i = 0; i < 5; i++) {
            banknotes[i] += banknotesCount[i];
            //amount += dict[i] * banknotesCount[i];
        }
    }
    
    public int[] withdraw(int amount) {
        int[] failed = new int[] {-1};
        /* if (this.amount < amount) {
            return failed;
        } */
        
        int[] withdraw = new int[5];
        for (int i = 4; i >= 0; i--) {
            if (amount / dict[i] == 0) {
                continue;
            }
            
            if (banknotes[i] >= (amount / dict[i])) {
                withdraw[i] += amount / dict[i];
                amount %= dict[i];
            } else {
                withdraw[i] = (int) banknotes[i];
                amount -= banknotes[i] * dict[i];
            }
        }
        
        if (amount != 0) {
            return failed;
        }
        
        for (int i = 0; i < 5; i++) {
            banknotes[i] -= withdraw[i];
        }
        // this.amount -= amount;
        
        // 成功了，减总额
        return withdraw;
    }
}
```

# 4. 2242. Maximum Score of a Node Sequence



