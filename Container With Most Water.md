# Container With Most Water

### Question link:

[https://leetcode.com/problems/container-with-most-water/](https://leetcode.com/problems/container-with-most-water/)

### Question Brief:

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return *the maximum amount of water a container can store*.

![Untitled](https://i.imgur.com/aohnE4R.png)

## Intuition behind the solution:

Breaking down the question a little bit, we understand that essentially we have to evaluate 

$$
max(Area(i,j))\  \forall (i,j) \in \{ (i,j ) \in [0,n) : i\ne j \}
$$

where,

$$
Area(i,j) = (j-i).min(height[j],height[i])
$$

we can say that `Area(i,j)` depends on two factors. `j - i` and minimum of `height[j]` and `height[i]`. and we have to find the `i` and `j` which will maximize these two factors combined.

Now,

If we use two pointers, one at the starting of the array `( i )` and one at the ending of the array `(j)` it would help us maximize the first factor, that is `(j-i)`.

we can increment `i` or decrement `j` for our next iteration.

So we have two choices. increment `i` or decrement `j`. what should we do?

notice what happens when I change (increment or decrement) the pointer that points to the maximum of `height[j]` and `height[i]`.

- $min(height[j],height[i])$ will **********************************never increase********************************** as this is limited my the minimum of  `height[j]` and `height[i]` and we left that unchanged.
- $(j-i)$ will ****************always**************** decrease. ********************************

Hence changing the pointer pointing to the maximum of the heights will ************************************************************never result in the optimal solution.************************************************************  

So what’s the winning move? changing the pointer that points to the minimum of the heights?

As it turn out, yes. But we must prove that it indeed will give us the optimal solution.

**Some Assumptions:**

- If we visit `i` and `j` , we calculate the `Area(i,j)` and store it somewhere.
- we stop next iterations if `j` becomes less than `i`.
- The optimal value of `i` and `j` that maximizes `Area(i,j)` is `i_optimal` and `j_optimal`.

Based on these assumptions, we only have one thing to prove.

`i` **and** `j` will be `i_optimal` **and** `j_optimal` respectively once during our iterations.

### Proof:

We can be assured that either `i` **or** `j` will point to `i_optimal` **or** `j_optimal` once during our iteration,

Let’s say that there’s a time during our iteration that `j = j_optimal`

Now `i` can be in three places:

- `i` is currently less than `i_optimal`
- `i` is currently equal to  `i_optimal`
- `i` is currently greater than  `i_optimal`

Let’s tackle the first two, the third revisited later.

************First:************ `i` is currently less than `i_optimal`

**********Claim:********** if `i` is currently less than `i_optimal` all the heights from `height[i]` to  `height[i_optimal]` are less than `height[j]` which will make us increment `i` till `i=i_optimal` .

**Proof :** This is a simple proof by contradiction, if there exists an index (say `i_cont`) that is less than `i_optimal` and height of that index is greater than `height[j]` . Then `Area(i_cont,j_optimal)` will come out to be greater than `Area(i_optimal,j_optimal)` . Hence this is not possible.

************Second:************ `i` is currently equal to `i_optimal`

We have nothing to do here, we visited `i_optimal` **and** `j_optimal` in this case.

Let’s look back and see what we have proved so far.

************************Statement 1:************************

When `j = j_optimal` if `i` is currently less than or equal to `i_optimal` , `i` will eventually be incremented to be `i_optimal`.

Using the same logic we can argue that,

********Statement 2:********

When  `i = i_optimal` if `j` is currently greater than or equal to `j_optimal` , `j` will eventually be decremented to be `j_optimal`.
With ************************Statement 1************************ and ********Statement 2******** . we can say that The **********Third********** case (`i` is currently greater than  `i_optimal`) is not possible, because for `i` to be  greater than `i_optimal` , it has to cross it, or it must become `i_optimal` first, and when than happens, we can use **************************Statement 2**************************  to prove that `j` will become `j_optimal`.

**************************Hence Proved.**************************

Here’s the code, using the iteration technique discussed above

```python
def maxArea(self, h):
        i = 0
        j = len(h)-1
        area = 0
        while(i < j):
            area = max(area, (j-i)*min(h[i],h[j]))
            if(h[i] < h[j]):
                i+=1
            else:
                j-=1
        

        return area
```