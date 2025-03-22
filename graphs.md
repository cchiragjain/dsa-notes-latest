- [323. Number of Connected Components in an Undirected Graph](#323-number-of-connected-components-in-an-undirected-graph)
  - [Approach](#approach)
  - [Code](#code)
- [547. Number of Provinces](#547-number-of-provinces)
  - [Approach](#approach-1)
  - [Code](#code-1)

# [323. Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/description/)

You have a graph of <code>n</code> nodes. You are given an integer <code>n</code> and an array <code>edges</code> where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the graph.

Return the number of connected components in the graph.

**Example 1:**
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg" style="width: 382px; height: 222px;">

```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```

**Example 2:**
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg" style="width: 382px; height: 222px;">

```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
```

**Constraints:**

- <code>1 <= n <= 2000</code>
- <code>1 <= edges.length <= 5000</code>
- <code>edges[i].length == 2</code>
- <code>0 <= a<sub>i</sub> <= b<sub>i</sub> < n</code>
- <code>a<sub>i</sub> != b<sub>i</sub></code>
- There are no repeated edges.

## Approach

- **Pattern: Basic DFS**
- Each dfs call can traverse its own graph independently on its own. So each component basically is a single dfs call only. So we can keep track of number of dfs calls.
- T.C => O(V + E) S.C => O(V + E)

## Code

```cpp
class Solution {
private:
    void dfs(int node, vector<vector<int>>& adjList, vector<bool>& visited) {
        visited[node] = true;

        for (auto& neighbour : adjList[node]) {
            if (!visited[neighbour]) {
                dfs(neighbour, adjList, visited);
            }
        }
    }

    // T.C => O(V + E) dfs call for graph and building adjList
    // S.C => O(V + E) for storing visited and adjList
public:
    int countComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adjList(n);
        vector<bool> visited(n);
        int count = 0; // keeps track of how many times dfs was called for the
                       // entire graph
        // in a single dfs call it will visit its entire graph component
        // next call will only be made for the not visited node from the next
        // component

        // create adj list for the undirected graph
        for (auto& edge : edges) {
            int u = edge[0];
            int v = edge[1];

            adjList[u].push_back(v);
            adjList[v].push_back(u);
        }

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i, adjList, visited);
                count++;
            }
        }

        return count;
    }
};
```

# [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/description/)

There are <code>n</code> cities. Some of them are connected, while some are not. If city <code>a</code> is connected directly with city <code>b</code>, and city <code>b</code> is connected directly with city <code>c</code>, then city <code>a</code> is connected indirectly with city <code>c</code>.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an <code>n x n</code> matrix <code>isConnected</code> where <code>isConnected[i][j] = 1</code> if the <code>i^th</code> city and the <code>j^th</code> city are directly connected, and <code>isConnected[i][j] = 0</code> otherwise.

Return the total number of **provinces** .

**Example 1:**
<img alt="" src="https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg" style="width: 222px; height: 142px;">

```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

**Example 2:**
<img alt="" src="https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg" style="width: 222px; height: 142px;">

```
Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
```

**Constraints:**

- <code>1 <= n <= 200</code>
- <code>n == isConnected.length</code>
- <code>n == isConnected[i].length</code>
- <code>isConnected[i][j]</code> is <code>1</code> or <code>0</code>.
- <code>isConnected[i][i] == 1</code>
- <code>isConnected[i][j] == isConnected[j][i]</code>

## Approach

- **Pattern: Basic DFS**
- Same as above new thing was how to convert adj mat to adj list

## Code

```cpp
class Solution {
private:
    void dfs(int node, vector<bool>& visited, vector<vector<int>>& adjList){
        visited[node] = true;

        for(auto& neighbour : adjList[node]){
            if(!visited[neighbour]){
                dfs(neighbour, visited, adjList);
            }
        }
    }

public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        // given the adjacency matrix we can convert to adj list first
        int v = isConnected.size();
        vector<vector<int>> adjList(v);

        for(int i = 0; i < v; i++){
            for(int j = 0; j < v; j++){
                // if there exists an edge b/w i and j and i does not connect to j ( no self loop )
                if(isConnected[i][j] == 1 && i != j){
                    adjList[i].push_back(j);
                    adjList[j].push_back(i);
                }
            }
        }

        int countOfProvinces = 0;
        vector<bool> visited(v, false);

        // har ek not visited node ke liye dfs call karege
        // jabhi uska dfs khatam hoga matlab usne apne vaala pura graph explore kar lia h
        // yeh dfs se pehel loop isliye lagaya h kyuki graph connected nahi h
        // similar to finding number of connected components
        for(int i = 0; i < v; i++){
            // imagine if starting at a node then uska dfs call karte hi voh apna pura graph visit karleta h
            if(!visited[i]){
                dfs(i, visited, adjList);
                countOfProvinces++;
            }
        }

        return countOfProvinces;
    }
};
```
