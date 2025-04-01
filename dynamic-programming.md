- [Questions List](#questions-list)
- [When/what to use dp](#whenwhat-to-use-dp)
- [509. Fibonacci Number](#509-fibonacci-number)
  - [Approach](#approach)
  - [Code](#code)

# Questions List

# When/what to use dp

- When in question there are overlapping subproblems ( problem that is already solved is being solved again )
- When there is optimal substructure present ( choti problem + choti problem = solution to badi problem )
- Ways to solve
  - Recursion + Memoisation ( top down approach )
  - Loops + DS (bottom up approach)
    - Space Optimisation ( can be done for some problems to reduce space complexity required )

# [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/description/)

The <b>Fibonacci numbers</b>, commonly denoted <code>F(n)</code> form a sequence, called the <b>Fibonacci sequence</b>, such that each number is the sum of the two preceding ones, starting from <code>0</code> and <code>1</code>. That is,

```
F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.
```

Given <code>n</code>, calculate <code>F(n)</code>.

**Example 1:**

```
Input: n = 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.
```

**Example 2:**

```
Input: n = 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.
```

**Example 3:**

```
Input: n = 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
```

**Constraints:**

- <code>0 <= n <= 30</code>

## Approach

- Pattern:

## Code

```cpp
/*
    * Steps to solve dp problem with top down approach ( recursion + memorisation )
    * Create dp array ( can be 1d, 2d or 3d dimensions depend on number of changing parameter )
    * pass this to recursion function and store answer whenever it gets computed
    * on each call check if answer was already computed or not and return that
*/
class Solution {
private:
    int solveUsingMemorisation(int n, vector<int>& dp){
        if(n <= 1) return n;

        if(dp[n] != -1) return dp[n]; // if already pre computed return that only

        dp[n] = solveUsingMemorisation(n - 1, dp) + solveUsingMemorisation(n - 2, dp);
        return dp[n];
    }

public:
    int fib(int n) {
        vector<int> dp(n + 1, -1); // need to return the nth fib number

        int result = solveUsingMemorisation(n, dp);
        return result;
    }
};
```
