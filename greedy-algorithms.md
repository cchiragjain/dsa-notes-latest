- [Greedy Algorithm](#greedy-algorithm)
  - [when greedy works](#when-greedy-works)
    - [template](#template)
- [Questions List](#questions-list)
  - [N Meetings in one Room](#n-meetings-in-one-room)
    - [Code](#code)
      - [Custom comparator if needed](#custom-comparator-if-needed)
- [252. Meeting Rooms](#252-meeting-rooms)
  - [Approach](#approach)
  - [Code](#code-1)
- [Maximum Meetings in One Room](#maximum-meetings-in-one-room)
    - [COde](#code-2)
- [455. Assign Cookies](#455-assign-cookies)
  - [Approach](#approach-1)
  - [Code](#code-3)
- [860. Lemonade Change](#860-lemonade-change)
  - [Approach](#approach-2)
  - [Code](#code-4)
- [1710. Maximum Units on a Truck](#1710-maximum-units-on-a-truck)
  - [Approach](#approach-3)
  - [Code](#code-5)

# Greedy Algorithm

- Approach in which we choose the best option at that time without thinking about future consequeunces. Jo us time best lag rahi ho.
- Basically be lalchi. Example if agar 10 chocolates of different prices are there and we need to buy maximum chocolates then sabse pehli approach yehi hogi ki sabse sasti vaali le jo 50 mein sakti.
- There is no general pattern to solve and is more of practice/ can try this approach.

## when greedy works

- When there is maximise/ minimise in problem but future decisions are not affected by current choice. ( agar affect karte h toh then saare explore karne padege and use dp ). Known as greedy choice property.
- There is optimal substructure ( can be broken down into smaller problems )

### template

1. Sort karo ya kisi rule ke according elements ko priority do.

2. Har step pe best dikhnay wali option uthao.

3. Repeat till task khatam ho jaye.

# Questions List

## [N Meetings in one Room](https://www.geeksforgeeks.org/problems/n-meetings-in-one-room-1587115620/1)

![alt text](images/image-18.png)

- Pattern: Activity Selection
- T.C => O(n log n)

### Code

```cpp
class Solution {
  public:
    // Function to find the maximum number of meetings that can
    // be performed in a meeting room.
    int maxMeetings(vector<int>& start, vector<int>& end) {
        // sort pair of start and end time of meetings based on end time
        // so the earlies end time is at the start
        // if we pick greedily in order to complete most meetings
        // we should complete all those that ends early
        int count = 1;

        int n = start.size();

        vector<pair<int, int>> intervals(n);

        for(int i = 0; i < n; i++){
            intervals[i] = {end[i], start[i]};
        }

        sort(intervals.begin(), intervals.end()); // will sort on the basis of end
        // if cant push end as first then need to use custom comparator

        int lastMeetingEndTime = intervals[0].first; // will need to complete the first meeting

        for(int i = 1; i < n; i++){
            // if start time of next meeting is greater than previous end
            // this meeting can be completed
            if(intervals[i].second > lastMeetingEndTime) {
                count++;
                lastMeetingEndTime = intervals[i].first;
            }
        }

        return count;
    }
};
```

#### Custom comparator if needed

- will sort in ascending order based on second value of the pairs provided

```cpp
static bool cmp(pair<int, int> a, pair<int, int> b){
  return a.second < b.second;
}
```

# [252. Meeting Rooms](https://leetcode.com/problems/meeting-rooms/description/)

Given an array of meeting time <code>intervals</code>where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, determine if a person could attend all meetings.

**Example 1:**

```
Input: intervals = [[0,30],[5,10],[15,20]]
Output: false
```

**Example 2:**

```
Input: intervals = [[7,10],[2,4]]
Output: true
```

**Constraints:**

- <code>0 <= intervals.length <= 10^4</code>
- <code>intervals[i].length == 2</code>
- <code>0 <= start<sub>i</sub> <end<sub>i</sub> <= 10^6</code>

## Approach

- Pattern: activity selection

## Code

```cpp
class Solution {
public:
    static bool cmp(vector<int>& a, vector<int>& b){
        return a[1] < b[1];
    }

    bool canAttendMeetings(vector<vector<int>>& intervals) {
        int n = intervals.size();
        if(n == 0) return true;

        sort(intervals.begin(), intervals.end(), cmp);
        int lastCompletedMeetingTime = intervals[0][1];

        for(int i = 1; i < n; i++){
            if(!(intervals[i][0] >= lastCompletedMeetingTime)) {
                return false;
            }
            lastCompletedMeetingTime = intervals[i][1];
        }

        return true;
    }
};
```

# [Maximum Meetings in One Room](https://www.geeksforgeeks.org/problems/maximum-meetings-in-one-room/1)

![alt text](images/image-19.png)

- Pattern: activity selection
- Check how to handle duplicate start times

### COde

```cpp
class Solution{
public:
    vector<int> maxMeetings(int N,vector<int> &start,vector<int> &end){
        // sort pair of start and end time of meetings based on end time
        // so the earlies end time is at the start
        // if we pick greedily in order to complete most meetings
        // we should complete all those that ends early
        vector<int> result;

        int n = start.size();
        unordered_map<int, int> mp;

        vector<pair<int, pair<int, int>>> intervals(n);

        for(int i = 0; i < n; i++){
            intervals[i] = {end[i], {start[i], i + 1}};
        }

        sort(intervals.begin(), intervals.end()); // will sort on the basis of end
        // if cant push end as first then need to use custom comparator

        int lastMeetingEndTime = intervals[0].first; // will need to complete the first meeting
        result.push_back(intervals[0].second.second); // first meeting will always be completed

        for(int i = 1; i < n; i++){
            // if start time of next meeting is greater than previous end
            // this meeting can be completed
            if(intervals[i].second.first > lastMeetingEndTime) {
                result.push_back(intervals[i].second.second);
                lastMeetingEndTime = intervals[i].first;
            }
        }

        sort(result.begin(), result.end());
        return result;
    }
};
```

# [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child <code>i</code> has a greed factor <code>g[i]</code>, which is the minimum size of a cookie that the child will be content with; and each cookie <code>j</code> has a size <code>s[j]</code>. If <code>s[j] >= g[i]</code>, we can assign the cookie <code>j</code> to the child <code>i</code>, and the child <code>i</code> will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Example 1:**

```
Input: g = [1,2,3], s = [1,1]
Output: 1
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3.
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
```

**Example 2:**

```
Input: g = [1,2], s = [1,2,3]
Output: 2
Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2.
You have 3 cookies and their sizes are big enough to gratify all of the children,
You need to output 2.
```

**Constraints:**

- <code>1 <= g.length <= 3 \* 10^4</code>
- <code>0 <= s.length <= 3 \* 10^4</code>
- <code>1 <= g[i], s[j] <= 2^31 - 1</code>

**Note:** This question is the same as <a href="https://leetcode.com/problems/maximum-matching-of-players-with-trainers/description/" target="_blank"> 2410: Maximum Matching of Players With Trainers.</a>

## Approach

- T.C => O(n log n + m Log m)
- The least greedy children should be satisfied with the smallest size cookies if it is within there greed.

## Code

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        // sort both greed and size so that children with the least greed have the small size cookies
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());

        int count = 0;
        int i = 0;
        int j = 0;

        while(i < g.size() && j < s.size()){
            if(g[i] <= s[j]){
                count++;
                i++;
                j++;
            } else{
                j++; // this cookie size can not be assigned since more i will be larger values of greed
            }
        }

        return count;
    }
};
```

# [860. Lemonade Change](https://leetcode.com/problems/lemonade-change/description/)

At a lemonade stand, each lemonade costs <code>$5</code>. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a <code>$5</code>, <code>$10</code>, or <code>$20</code> bill. You must provide the correct change to each customer so that the net transaction is that the customer pays <code>$5</code>.

Note that you do not have any change in hand at first.

Given an integer array <code>bills</code> where <code>bills[i]</code> is the bill the <code>i^th</code> customer pays, return <code>true</code> if you can provide every customer with the correct change, or <code>false</code> otherwise.

**Example 1:**

```
Input: bills = [5,5,5,10,20]
Output: true
Explanation:
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.
```

**Example 2:**

```
Input: bills = [5,5,10,10,20]
Output: false
Explanation:
From the first two customers in order, we collect two $5 bills.
For the next two customers in order, we collect a $10 bill and give back a $5 bill.
For the last customer, we can not give the change of $15 back because we only have two $10 bills.
Since not every customer received the correct change, the answer is false.
```

**Constraints:**

- <code>1 <= bills.length <= 10^5</code>
- <code>bills[i]</code> is either <code>5</code>, <code>10</code>, or <code>20</code>.

## Approach

- T.C: O(N)

## Code

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int fiveCount = 0;
        int tenCount = 0;

        for(int i = 0; i < bills.size(); i++){
            if(bills[i] == 5) {
                // no change is needed to be given back
                // just incrememnt
                fiveCount++;
            } else if(bills[i] == 10){
                // can only be possible if we have atleast one five dollar bill
                if(fiveCount > 0){
                    fiveCount--;
                    tenCount++;
                } else {
                    return false;
                }
            } else {
                // if bill is 20 then there are 2 cases
                // either 10 + 5 or 5 + 5 + 5 needs to be returned
                // but we need max 5 so best to first check 10 dollar bills
                if(tenCount > 0 && fiveCount > 0){
                    tenCount--;
                    fiveCount--;
                } else if(fiveCount >= 3){
                    fiveCount -= 3;
                } else {
                    return false;
                }
            }
        }

        return true;
    }
};
```

# [1710. Maximum Units on a Truck](https://leetcode.com/problems/maximum-units-on-a-truck/description/)

You are assigned to put some amount of boxes onto **one truck** . You are given a 2D array <code>boxTypes</code>, where <code>boxTypes[i] = [numberOfBoxes<sub>i</sub>, numberOfUnitsPerBox<sub>i</sub>]</code>:

- <code>numberOfBoxes<sub>i</sub></code> is the number of boxes of type <code>i</code>.
- <code>numberOfUnitsPerBox<sub>i</sub></code><sub> </sub>is the number of units in each box of the type <code>i</code>.

You are also given an integer <code>truckSize</code>, which is the **maximum** number of **boxes** that can be put on the truck. You can choose any boxes to put on the truck as long as the numberof boxes does not exceed <code>truckSize</code>.

Return the **maximum** total number of **units** that can be put on the truck.

**Example 1:**

```
Input: boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
Output: 8
Explanation: There are:
- 1 box of the first type that contains 3 units.
- 2 boxes of the second type that contain 2 units each.
- 3 boxes of the third type that contain 1 unit each.
You can take all the boxes of the first and second types, and one box of the third type.
The total number of units will be = (1 * 3) + (2 * 2) + (1 * 1) = 8.
```

**Example 2:**

```
Input: boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
Output: 91
```

**Constraints:**

- <code>1 <= boxTypes.length <= 1000</code>
- <code>1 <= numberOfBoxes<sub>i</sub>, numberOfUnitsPerBox<sub>i</sub> <= 1000</code>
- <code>1 <= truckSize <= 10^6</code>

## Approach

- T.C => O(n log n)

## Code

```cpp
class Solution {
private:
    // sorts in descending order on the basis of second element
    static bool cmp(vector<int>& a, vector<int>& b){
        return a[1] > b[1];
    }
public:
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        // sort box types on the basis of units so that the first boxes we put on the truck have the maximum units in order to maximise units
        sort(boxTypes.begin(), boxTypes.end(), cmp);

        int maxUnits = 0;

        for(int i = 0; i < boxTypes.size(); i++){
            int boxes = boxTypes[i][0];
            int unitsInABox = boxTypes[i][1];

            // will auto stop if at any time truckSize becomes negative since boxes count is >= 1
            if(boxes <= truckSize){
                truckSize -= boxes;
                maxUnits += (boxes * unitsInABox);
            } else if(truckSize > 0){
                // ex. if size remaining was 2 and we had 4 boxes
                // then can still use 2 boxes to fill up the truck
                int boxesUsed = truckSize;
                truckSize -= boxesUsed;
                maxUnits += (boxesUsed * unitsInABox);
            }
        }

        return maxUnits;
    }
};
```
