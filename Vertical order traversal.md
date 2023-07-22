# Vertical order traversal of a Binary tree

### Question Link:

[https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/)

## Intuition behind the solution

![Screenshot 2023-07-22 at 2.24.34 PM.png](https://i.imgur.com/5jjoMUc.png)

We have to group nodes at each vertical line(refer to the dotted line in the diagram) together staring from the leftmost vertical line and going towards the rightmost vertical line.

on top of this we have other rules:

- if there are multiple nodes in a group, we sort the nodes in top-to-bottom order i.e,. increasing order of their height. So in group [1,5,6] of line 0. [1] is placed before [5] and [6].
- if some nodes have the same height like [5] and [6] in the above example. sort them in increasing order of their values. so [5] comes before [6].

reading the question it should be apparent the we need to keep track of two things for each node.

- the node’s height
- the vertical line that the node falls into.

both of these are fairly simple to do.

For any node $i$

$$
height(i) = \begin{cases} 0 && parent(i)=NULL\\height(parent(i))+1 && otherwise &&\end{cases}
$$

To keep track of the vertical line that the node falls into, we need to do different things depending on wether we are visiting the left child or the right child of the current node.

For any node $i$

$$
verticle(leftchild(i))= verticle(i)-1 
$$

$$
verticle(rightchild(i))= verticle(i)+1 
$$

Where

$$
verticle(i) = 0\ \ \ \ if \ \ \ parent(i)=NULL
$$

Using the above equations our job is almost done, we just need to visit all nodes once and then form groups using the height and the vertical data of each node.

Next steps:

- Visit all nodes once (BFS or DFS if doesn’t really matter ).
- store all the nodes in an array or some equivalent data structure.
- sort the array using a **************************custom comparator************************** to satisfy the the two rules of ordering we talked about earlier
- done.

In the code below, I’ve used BFS and priority queue with a custom comparator to get the job done.

 

```cpp
vector<vector<int>> verticalTraversal(TreeNode* root) {
        struct customLess{
            bool operator()(const pair<pair<int,int>,TreeNode*>& l,const pair<pair<int,int>,TreeNode*>& r){
                if(l.first.first == r.first.first){
                    if(l.first.second == r.first.second){
                        return l.second->val > r.second->val;
                    }
                    return l.first.second > r.first.second;
                }
                return l.first.first > r.first.first;
            }
        };
        priority_queue<pair<pair<int,int>,TreeNode*>, vector<pair<pair<int,int>,TreeNode*>>, customLess > pq;
        queue<pair<pair<int,int>,TreeNode*>> bfs;
        vector<vector<int>> ans(1,vector<int>());
        bfs.push({{0,0},root});
        while(!bfs.empty()){
            TreeNode * n = bfs.front().second;
            int depth = bfs.front().first.second;
            int dev = bfs.front().first.first;
            pq.push(bfs.front());
            bfs.pop();
            if(n->left){
                bfs.push({{dev-1,depth+1},n->left});
            }
            if(n->right){
                bfs.push({{dev+1,depth+1},n->right});
            }
        }
        int curgrp = pq.top().first.first;
        int i = 0;
        while(!pq.empty()){
            int dev = pq.top().first.first;
            int data = (pq.top().second)->val;
            pq.pop();
            if(dev == curgrp){
                ans[i].push_back(data);
            }else{
                curgrp = dev;
                ans.push_back({data});
                i++;
            }
        }

        return ans;
    }
```