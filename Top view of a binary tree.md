# Top View of Binary Tree

### Question Link :-

[https://practice.geeksforgeeks.org/problems/top-view-of-binary-tree/1](https://practice.geeksforgeeks.org/problems/top-view-of-binary-tree/1)

## Intuition behind the solution

![DAEF8AF0-0644-408A-BCDE-3A7AC28D9F2A_1_201_a.jpeg](https://i.imgur.com/ax0gZxZ.jpg)

### Observe

We want all the nodes that would be visible if we viewed our tree from the top.

essentially we need a way to check wether a node would be visible from the top or not.

To do this we will keep track of the rightmost and leftmost nodes hat we have visited so far.

these rightmost and leftmost nodes dictate wether any nodes under them are visible from the top or not.

if a node occurs more on the right than the rightmost node till now, it will be visible from the top.

the same logic goes for the leftmost node.

so essentially our problem boils down to keeping track of the leftmost and rightmost node at each iteration.

Now, if you think harder, we don’t really need the rightmost nodes and the leftmost nodes, we just need to measure how right or left they are.

hence we will keep track of two variables `maxL` and `maxR` .

both variables will keep track of the “right-ness” and the “left-ness” of the rightmost and the leftmost nodes.

we will define “right-ness” of each node as

$$
rightness(i) = \begin{cases}       rightness(parent(i))+1 &  \\
      0 & parent(i)=NULL
   \end{cases}
$$

and we will define “left-ness” of each node as

$$
leftness(i) = \begin{cases}       leftness(parent(i))-1 &  \\
      0 & parent(i)=NULL
   \end{cases}
$$

if we come across any node during our **************************level order traversal.************************** whose right-ness is greater than `maxR` we will set `maxR` = right-ness our node, and include the node in our solution.

The same argument works for the left-ness of node i;

## The code

```cpp
/*
struct Node
{
    int data;
    Node* left;
    Node* right;
};
*/
vector<int> topView(Node *root){
    queue<pair<Node*,int>> q;
    q.push({root,0});
    int maxL = 0;
    int maxR = 0;
    vector<int> l;
    vector<int> r;
    r.push_back(root->data);
    while(!q.empty()){
        Node * n = q.front().first;
        int d = q.front().second;
        q.pop();
        if(d < maxL){
            maxL = d;
            l.push_back(n->data);
        }else if (d > maxR){
            maxR=d;
            r.push_back(n->data);
        }
        if(n->left){
            q.push({n->left,d-1});
        }
        if(n->right){
            q.push({n->right,d+1});
        }
    }
    reverse(l.begin(),l.end());
    l.insert(l.end(),r.begin(),r.end());
    return l;
}
```