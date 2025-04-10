- [Questions List](#questions-list)
- [When/what to use dp](#whenwhat-to-use-dp)
- [509. Fibonacci Number](#509-fibonacci-number)
  - [Approach](#approach)
  - [Code](#code)
- [When to apply dp/ recursion to solve a problem](#when-to-apply-dp-recursion-to-solve-a-problem)
  - [How to solve/ write recurrence relation](#how-to-solve-write-recurrence-relation)
- [70. Climbing Stairs](#70-climbing-stairs)
  - [Approach](#approach-1)
  - [Code](#code-1)
    - [Recursive](#recursive)
    - [Memoisation](#memoisation)
- [Frog Jump](#frog-jump)
  - [Code](#code-2)
- [Frog Jump with K Steps](#frog-jump-with-k-steps)
  - [Code](#code-3)
- [198. House Robber](#198-house-robber)
  - [Approach](#approach-2)
  - [Code](#code-4)
- [213. House Robber II](#213-house-robber-ii)
  - [Approach](#approach-3)
  - [Code](#code-5)
- [55. Jump Game](#55-jump-game)
  - [Approach](#approach-4)
  - [Code](#code-6)

# Questions List

# When/what to use dp

- When in question there are overlapping subproblems ( problem that is already solved is being solved again )
- When there is optimal substructure present ( choti problem + choti problem = solution to badi problem )
- Ways to solve
  - Recursion + Memoisation ( top down approach )
  - Loops + DS (bottom up approach)
    - Space Optimisation ( can be done for some problems to reduce space complexity required )
- Time complexity of recursion generally -> O(branches^height of tree) ( 2 ^ n => har jagah 2 option h fir unke 2 option h ... 2 _ 2 _ 2 ...)

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

- T.C => O(N) reduced from O(2^n)
- S.C => O(N) for both memoisation and tabulation but tabluation improves as it does not have recursion overhead
- S.C => O(1) for space optimised version
-

## Code

```cpp
/*
    * Steps to solve dp problem with top down approach ( recursion + memorisation ) ( goes from n -> 0 )
      * Create dp array ( can be 1d, 2d or 3d dimensions depend on number of changing parameter )
      * pass this to recursion function and store answer whenever it gets computed
      * on each call check if answer was already computed or not and return that

    * Steps to solve using bottom up approach ( iterative + tabulation ) ( builds solution from 0 to n)
      * Create dp array ( can be 1d, 2d or 3d dimensions depend on number of changing parameter )
      * Initialise dp array with base cases
      * Same logic as recursive and run loop while building solutions from 0 to n

    * Steps to space optimise
      * Check if a pattern exists in tabulation method and then work on that
*/
class Solution {
private:
    int solveUsingMemorisation(int n, vector<int>& dp){
        if(n <= 1) return n;

        if(dp[n] != -1) return dp[n]; // if already pre computed return that only

        dp[n] = solveUsingMemorisation(n - 1, dp) + solveUsingMemorisation(n - 2, dp);
        return dp[n];
    }

    int solveUsingTabulation(int n){
        if(n <= 1) return n; // base cases can causes issues if say n was 0 then dp will be of size 1 but we will accessing dp[1] etc.

        vector<int> dp(n + 1, -1);

        dp[0] = 0;
        dp[1] = 1;

        for(int i = 2; i <= n; i++){
            dp[i] = dp[i - 1] + dp[i - 2]; // building solutions using previous
        }

        return dp[n];
    }

    int solveUsingTabulationSpaceOptimised(int n){
        if(n <= 1) return n;

        int prev = 0;
        int curr = 1;
        int ans;

        // if we observe we only need the previous 2 values from the array in tabulation
        // can just store in variables instead
        for(int i = 2; i <= n; i++){
            ans = prev + curr;
            prev = curr;
            curr = ans;
        }

        return ans;
    }

public:
    int fib(int n) {
        vector<int> dp(n + 1, -1); // need to return the nth fib number

        int result = solveUsingMemorisation(n, dp);
        return result;
    }
};
```

# When to apply dp/ recursion to solve a problem

- Problems where need to explore all paths/ ways to figure something out. ex.
  - Problem asking for count the total number of ways.
  - Given multiple ways of performing a task, it is asked which way will yield the minimum or maximum output.
- ex.
  - The first characteristic that is common in DP problems is that the problem will ask for the optimum value (maximum or minimum) of something, or the number of ways there are to do something. For example:
    - **What is the minimum cost of doing...**
    - **What is the maximum profit from...**
    - **How many ways are there to do...**
    - **What is the longest possible...**
    - **Is it possible to reach a certain point...**
- **These are also possible that they may be greedy algo questions. So in that check with examples that whether if doing something now wont affect our future decisions. ex. in house robbber problem if we pick the highest house only in adjacent then may not be able to pick the ones with highest money.**
- **To summarize: if a problem is asking for the maximum/minimum/longest/shortest of something, the number of ways to do something, or if it is possible to reach a certain point, it is probably greedy or DP. in general, if the problem has constraints that cause decisions to affect other decisions, such as using one element prevents the usage of other elements, then we should consider using dynamic programming to solve the problem. These two characteristics can be used to identify if a problem should be solved with DP.**
- agar element pick karke baakion ka use kharab hoga then we should mostly use dp but baaki still available h then try greedy. if problem has above things.

## How to solve/ write recurrence relation

- Try to represent problem using indexes
- Do all possible stuff on that idx according to problem
- Count of all ways -> sum all stuff
- Minimum of all ways -> min all stuff
- maximum of all ways -> find max all stuff

# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

You are climbing a staircase. It takes <code>n</code> steps to reach the top.

Each time you can either climb <code>1</code> or <code>2</code> steps. In how many distinct ways can you climb to the top?

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

**Constraints:**

- <code>1 <= n <= 45</code>

## Approach

- Pattern: 1-d dp

## Code

### Recursive

- Similar to fibonacci only. f(n) = f(n - 1) + f(n - 2)
- Can also be done from 0 to n with first call of f(0) = f(1) + f(2) and so on with base condition at i == n return 1 and if i > n return 0.

```cpp
// think of n as index can go from n -> 0

class Solution {
public:
    // this function returns number of ways it takes to reach from n to 0
    int climbStairs(int n) {
        if(n == 0 || n == 1) return 1; // if we at 0 then only 1 way exists to reach 0 ( do nothing )
        // if we at 1 then can take 1 step

        // if climbStairs(n) return number of ways from n -> 0
        // then climbStairs(n - 1) will return number of ways from n - 1 -> 0
        int numberOfWaysIfWeTakeOneStep = climbStairs(n - 1);
        int numberOfWaysIfWeTakeTwoSteps = climbStairs(n - 2);

        // problem asks for count of all ways return sum
        return numberOfWaysIfWeTakeOneStep + numberOfWaysIfWeTakeTwoSteps;
    }
};
```

### Memoisation

```cpp
// think of n as index can go from n -> 0

class Solution {
private:
    int climbStairs(int n, vector<int>& dp){
        if(n == 0 || n == 1) return 1;

        if(dp[n] != -1) return dp[n];

        dp[n] = climbStairs(n - 1, dp) + climbStairs(n - 2, dp);
        return dp[n];
    }

public:
    // similar to fibonacci sequence
    // can be further optimised to tabulation -> space optimised with a loop
    int climbStairs(int n) {
        vector<int> dp(n + 1, -1);

        return climbStairs(n, dp);
    }

    /* recursive
    // this function returns number of ways it takes to reach from n to 0
    int climbStairs(int n) {
        if(n == 0 || n == 1) return 1; // if we at 0 then only 1 way exists to reach 0 ( do nothing )
        // if we at 1 then can take 1 step

        // if climbStairs(n) return number of ways from n -> 0
        // then climbStairs(n - 1) will return number of ways from n - 1 -> 0
        int numberOfWaysIfWeTakeOneStep = climbStairs(n - 1);
        int numberOfWaysIfWeTakeTwoSteps = climbStairs(n - 2);

        // problem asks for count of all ways return sum
        return numberOfWaysIfWeTakeOneStep + numberOfWaysIfWeTakeTwoSteps;
    }
    */
};
```

# [Frog Jump](https://www.geeksforgeeks.org/problems/geek-jump/1?utm_source=youtube&utm_medium=collab_striver_ytdescription&utm_campaign=geek-jump)

![alt text](images/image-16.png)

## Code

- Steps followed same as above

  - Convert to use indexes
  - do stuff
  - find min of all stuff

- Can also do space optimisation similar to fibonacci
- T.C => O(N) and S.C => O(N)

```cpp
class Solution {
private:
    int minCostTabulation(vector<int>& height, int n){
        vector<int> dp(n + 1, -1);

        dp[0] = 0;
        dp[1] = abs(height[1] - height[0]);

        for(int i = 2; i < n; i++){
            dp[i] = min(dp[i - 1] + abs(height[i] - height[i - 1]),
                        dp[i - 2] + abs(height[i] - height[i - 2]));
        }

        return dp[n];
    }

    int minCostMemoised(vector<int>& height, vector<int>& dp, int currentStep){
        if(currentStep == 0) return 0; // if at 0 step then to go to 0th step
        // it will take 0 cost
        if(dp[currentStep] != -1) return dp[currentStep];

        // cost till here + the cost to go from n - 1 -> n
        int costToTakeOneStep = minCostMemoised(height, dp, currentStep - 1) + abs(height[currentStep] - height[currentStep - 1]);

        // can only take 2 steps if not at 1 or 0
        int costToTakeTwoSteps = INT_MAX;

        if(currentStep > 1) {
            costToTakeTwoSteps = minCostMemoised(height, dp, currentStep - 2) + abs(height[currentStep] - height[currentStep - 2]);
        }

        // asssings and then returns
        return dp[currentStep] = min(costToTakeOneStep, costToTakeTwoSteps);
    }

    int minCost(vector<int>& height, int currentStep){
        if(currentStep == 0) return 0; // if at 0 step then to go to 0th step
        // it will take 0 cost

        // cost till here + the cost to go from n - 1 -> n
        int costToTakeOneStep = minCost(height, currentStep - 1) + abs(height[currentStep] - height[currentStep - 1]);

        // can only take 2 steps if not at 1 or 0
        int costToTakeTwoSteps = INT_MAX;

        if(currentStep > 1) {
            costToTakeTwoSteps = minCost(height, currentStep - 2) + abs(height[currentStep] - height[currentStep - 2]);
        }

        return min(costToTakeOneStep, costToTakeTwoSteps);
    }

  public:
    int minCost(vector<int>& height) {
        int n = height.size();
        int top = n - 1;
        vector<int> dp(n, -1);

        return minCostTabulation(height, n);
    }
};
```

# [Frog Jump with K Steps](https://www.geeksforgeeks.org/problems/minimal-cost/1?utm_source=youtube&utm_medium=collab_striver_ytdescription&utm_campaign=minimal-cost)

![alt text](images/image-17.png)

## Code

- T.C => O(N \* K) for memo and tabulation but O(k ^ N) for recursion ( k options for n choices )
- same as above but at each step we can take k steps so need to check if current - k is possible or not

```cpp
class Solution {
private:
    int minimizeCostMemo(int k, vector<int>& arr,vector<int>& dp, int currentStep){
        if(currentStep == 0) return 0;

        if(dp[currentStep] != -1) return dp[currentStep];

        int minimumCost = INT_MAX;

        for(int i = 1; i <= k; i++) {
            if(currentStep - i >= 0) {
                minimumCost = min(
                        minimizeCostMemo(k, arr, dp, currentStep - i) +
                        abs(arr[currentStep] - arr[currentStep - i]),
                        minimumCost
                    );
            }
        }

        return dp[currentStep] = minimumCost;
    }

    int minimizeCostTabulation(int k, vector<int>& arr){
        int n = arr.size();

        vector<int> dp(n + 1, -1);
        dp[0] = 0;
        dp[1] = abs(arr[1] - arr[0]);

        for(int i = 2; i < n; i++){
            int minCost = INT_MAX;
            for(int j = 1; j <= k; j++){
                if(i - j >= 0){
                    minCost = min(dp[i - j] + abs(arr[i] - arr[i - j]), minCost);
                }
            }

            dp[i] = minCost;
        }

        return dp[n - 1];
    }

    int minimizeCost(int k, vector<int>& arr, int currentStep){
        if(currentStep == 0) return 0;

        int minimumCost = INT_MAX;

        for(int i = 1; i <= k; i++) {
            if(currentStep - i >= 0) {
                minimumCost = min(
                        minimizeCost(k, arr, currentStep - i) +
                        abs(arr[currentStep] - arr[currentStep - i]),
                        minimumCost
                    );
            }
        }

        return minimumCost;
    }
  public:
    int minimizeCost(int k, vector<int>& arr) {
        int n = arr.size();
        vector<int> dp(n + 1, -1);
        return minimizeCostTabulation(k, arr);
    }
};
```

# [198. House Robber](https://leetcode.com/problems/house-robber/description/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and <b>it will automatically contact the police if two adjacent houses were broken into on the same night</b>.

Given an integer array <code>nums</code> representing the amount of money of each house, return the maximum amount of money you can rob tonight <b>without alerting the police</b>.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

**Constraints:**

- <code>1 <= nums.length <= 100</code>
- <code>0 <= nums[i] <= 400</code>

## Approach

- Pattern: 1-d dp
- T.C => O(2^N) reduced to O(N) and S.C => O(1) using space optimisation

## Code

```cpp
class Solution {
private:
    int rob(vector<int>& nums, int i){
        if(i == 0) return nums[0]; // no negative numbers are present since cost
        // so should always pick this

        if(i < 0) return 0; // out of bounds case may happen say when at house 1 and call pick

        // can not pick adjacent elements so if we are picking it cant pick the adjacent
        int pick = nums[i] + rob(nums, i - 2);
        // if not picking consider current house as 0 and pick adjacent
        int notPick = 0 + rob(nums, i - 1);

        return max(pick, notPick);
    }

    int robMemo(vector<int>& nums, vector<int>& dp, int i){
        if(i == 0) return nums[0]; // no negative numbers are present since cost
        // so should always pick this

        if(i < 0) return 0; // out of bounds case may happen say when at house 1 and call pick

        if(dp[i] != -1) return dp[i];

        // can not pick adjacent elements so if we are picking it cant pick the adjacent
        int pick = nums[i] + robMemo(nums, dp, i - 2);
        // if not picking consider current house as 0 and pick adjacent
        int notPick = 0 + robMemo(nums, dp, i - 1);

        return dp[i] = max(pick, notPick);
    }

    int robTab(vector<int>& nums){
        int n = nums.size();
        vector<int> dp(n, -1);

        dp[0] = nums[0];

        if(n > 1)
            dp[1] = max(nums[0], nums[1]);

        for(int i = 2; i < n; i++){
            int pick = nums[i] + dp[i - 2];
            int notPick = 0 + dp[i - 1];
            dp[i] = max(pick, notPick);
        }

        return dp[n - 1];
    }

    int robSpaceOptimised(vector<int>& nums){
        int n = nums.size();

        int prev = nums[0];
        int curr = nums[0];

        if(n > 1)
            curr = max(nums[0], nums[1]);

        for(int i = 2; i < n; i++){
            int pick = nums[i] + prev;
            int notPick = 0 + curr;

            int curri = max(pick, notPick);
            prev = curr;
            curr = curri;
        }

        return curr;
    }

public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n + 1, -1);
        // return rob(nums, n - 1);
        // return robMemo(nums, dp, n - 1);
        return robSpaceOptimised(nums);
    }
};
```

# [213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and<b>it will automatically contact the police if two adjacent houses were broken into on the same night</b>.

Given an integer array <code>nums</code> representing the amount of money of each house, return the maximum amount of money you can rob tonight **without alerting the police** .

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3:**

```
Input: nums = [1,2,3]
Output: 3
```

**Constraints:**

- <code>1 <= nums.length <= 100</code>
- <code>0 <= nums[i] <= 1000</code>

## Approach

- Pattern:

## Code

```cpp
class Solution {
    // taken from house robber
private:
    int robSpaceOptimised(vector<int>& nums){
        int n = nums.size();

        int prev = nums[0];
        int curr = nums[0];

        if(n > 1)
            curr = max(nums[0], nums[1]);

        for(int i = 2; i < n; i++){
            int pick = nums[i] + prev;
            int notPick = 0 + curr;

            int curri = max(pick, notPick);
            prev = curr;
            curr = curri;
        }

        return curr;
    }

// the only difference in this problems as compared to house robber 1 is that
// first and last elements are also considered elements
// so answer can either be from 0 to n - 2 or 1 to n - 1 in array taking maximum from both
public:
    int rob(vector<int>& nums) {
        int n = nums.size();

        if(n == 1) return nums[0];

        vector<int> temp1, temp2;

        for(int i = 0; i < n; i++){
            if(i != 0) temp1.push_back(nums[i]);
            if(i != n - 1) temp2.push_back(nums[i]);
        }

        return max(robSpaceOptimised(temp1), robSpaceOptimised(temp2));
    }
};
```

# [55. Jump Game](https://leetcode.com/problems/jump-game/description/)

You are given an integer array <code>nums</code>. You are initially positioned at the array's **first index** , and each element in the array represents your maximum jump length at that position.

Return <code>true</code> if you can reach the last index, or <code>false</code> otherwise.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

**Constraints:**

- <code>1 <= nums.length <= 10^4</code>
- <code>0 <= nums[i] <= 10^5</code>

## Approach

- Pattern:

## Code

```cpp
class Solution {
private:
    bool canJump(vector<int>& nums, int currentStep, int n, vector<int>& memo){
        if(currentStep == n - 1){
            return true;
        } else if(currentStep >= n) {
            return false;
        }

        // 0 and 1 can be used as false and true as well
        if(memo[currentStep] != -1) return memo[currentStep];

        int maxJump = nums[currentStep];
        // if standing at 2 then can take a jump of 1 or 2
        // trying both and checking which one reaches first
        for(int i = 1; i <= maxJump; i++){
            if(canJump(nums, currentStep + i, n, memo)) {
                memo[currentStep + i] = 1;
                return true;
            }
        }

        memo[currentStep] = 0;
        // if starting from this not possible then return false
        return memo[currentStep];
    }

    bool canJumpTabulation(vector<int>& nums, int n){
        vector<int> dp(n, -1);
        dp[n - 1] = 1; // if at last index can always reach there ( already there )

        // now check from behind if doing nums[i] jump can reach an index from where we can reach end then using i we can also reach end

        for(int i = n - 2; i >= 0; i--){
            int maxJump = nums[i];
            bool canReach = false;
            for(int j = 1; j <= maxJump; j++){
                int newPosition = i + j; // current + jump size
                if(newPosition < n && dp[newPosition] == 1){
                    dp[i] = 1;
                    canReach = true;
                    // if was able to reach through any then dont need to check for others
                    break;
                }
            }
            // will only reach here if the loop did not break then was not able to find any position
            if(canReach == false) dp[i] = 0;
        }

        return dp[0] == 1;
    }

public:
    bool canJump(vector<int>& nums) {
        int currentStep = 0;
        int n = nums.size();
        // -1 means not solved
        // 0 means cant reach
        // 1 means can reach
        // vector<int> memo(n, -1);

        // return canJump(nums, currentStep, n, memo);
        // return canJumpTabulation(nums, n);

        // if we see the dp tabulation only need the last position where we can reach
        int lastPos = n - 1; // position of last good index from where we can reach end
        for(int currentPos = n - 2; currentPos >= 0; currentPos--){
            if(nums[currentPos] + currentPos >= lastPos) {
                lastPos = currentPos; // we can reach this
            }
        }

        return lastPos == 0;
    }
};
```
