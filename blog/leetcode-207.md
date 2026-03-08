
# leetcode 207. 课程表 刷题笔记
**文章标签：** leetcode、笔记、算法

你这个学期必须选修 numCourses 门课程，记为 0 到 numCourses - 1 。
在选修某些课程之前需要一些先修课程。 先修课程按数组 prerequisites 给出，其中 prerequisites[i] = [ai, bi] ，表示如果要学习课程 ai 则 必须 先学习课程 bi 。
例如，先修课程对 [0, 1] 表示：想要学习课程 0 ，你需要先完成课程 1 。
请你判断是否可能完成所有课程的学习？如果可以，返回 true ；否则，返回 false 。

**示例 1：**
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。

**示例 2：**
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。

**提示：**
- 1 <= numCourses <= 2000
- 0 <= prerequisites.length <= 5000
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- prerequisites[i] 中的所有课程对 互不相同

---

### 题目分析
就是找环？其实也可以按照拓扑排序来做，拓扑排序就是从入度为0出发，一个一个把当前节点剔除出去，对应的节点的入度减一。

### 拓扑排序代码 (BFS)

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>>a(numCourses);
        vector<int>in(numCourses,0);
        queue<int>q;
        int cnt=0;
        for(auto i:prerequisites){
            a[i[1]].push_back(i[0]);
            in[i[0]]++;
        }
        for(int i=0;i<numCourses;i++){
            if(!in[i])q.push(i);
        }
        while(!q.empty()){
            int t=q.front();
            q.pop();
            cnt++;
            for(int next:a[t]){
                in[next]--;
                if(!in[next])q.push(next);
            }
        }
        return cnt==numCourses;
    }
};

```

**纯 DFS 找环代码**

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>>a(numCourses);
        vector<int>vis(numCourses,0);
        for(auto i:prerequisites){
            a[i[1]].push_back(i[0]);
        }
        for(int i=0;i<numCourses;i++){
            if(vis[i]==0){
                if(dfs(a,vis,i)){
                    return false;
                }
            }
        }
        return true;
    }
    bool dfs(vector<vector<int>>&a,vector<int>&vis,int cur){
        vis[cur]=1;
        for(int next:a[cur]){
            if(vis[next]==0){
                if(dfs(a,vis,next)){
                    return true;
                }
            }else if(vis[next]==1){
                return true;
            }else{
 
            }
        }
        vis[cur]=2;
        return false;
    }
};

```