# Monsters

### Question Link:

[https://codeforces.com/contest/1849/problem/B](https://codeforces.com/contest/1849/problem/B)

## Intuition

You’ve read the question, we have to print the order in which the monsters die.

Now riddle me this.

Do we really care about who Monocarp hits? 

Well yes, if the monster that he hit dies then we’ll have to report that.

but what if the monster that he hits doesn’t die?

Hold on to that thought for a sec.

Let’s make another observation, any monster that Monocarp kills must have health ≤ k.

that’s pretty self explanatory.

Now since Monocarp hits only the monster with that maximum health. If there’s any monster the the any with health ≥ k at any point of time, Monocarp will eventually hit that monster until his health becomes ≤ k.

So, think for a second, will Monocarp be able to kill ******any****** monster until  the HP of all the monsters falls below k?

No, right?

Ok, that’s nice but how do we tell in what order Monocarp will kill them?

since Monocarp always kills the monster with the highest HP, the order ultimately depends on what HP the monsters have until they are a single hit away from death.

The monster with the highest HP remaining when they are a single hit away will die first, then the second highest so on.

confused?

here’s an example:

let $k = 3$

and $healths =[2,8,3,5]$

so,

healths just before the each monster dies will be

$$
health = [2,3,3,2]
$$

hence monster 2 dies then 3 then 1 and then 5.

Here’s the implementation in code.

```cpp
#include <iostream>
#include<queue>
#include<algorithm>
using namespace std;

int main() {
	int t;
	cin >> t;
	for(int _ = 0 ; _ < t ;_ ++){
	    int n,k;
	    cin >> n >> k;
	    struct {
	        bool operator()(const pair<int,int> & l,const pair<int,int> & r ){
	            if(l.second == r.second){
                    return l.first < r.first;
                }
                return l.second > r.second;
	        }
	    }customLess;
        vector<pair<int,int>> v(n,{0,0});
        for(int i = 0 ; i< n; i++){
            v[i].first=i;
            cin >> v[i].second;
            v[i].second = v[i].second%k;
            if(v[i].second == 0){
                v[i].second = k;
            } 
        }
        sort(v.begin(),v.end(),customLess);
        for(auto p : v){
            cout << p.first +1 << " ";
        }

	    cout << '\n';
	}
	return 0;
}
```