---
tags: CP
---

## Sandwich

<https://codebreaker.xyz/problem/sandwich>

This editorial is super long overdue, writing it now for completeness.

### Easier Version

For convenience, $A_0 = A_{n+1} = \infty$.

Firstly, let us understand how to solve the problem for all queries with $(x_i,y_i) = (1,i)$.

We will need to find how many ranges $(l,r)$ are a sandwich for a fixed $r$.

Let $S_r$ be the set of $l<r$ such that $A_l \geq \max(A_{l+1}, A_{l+1}, \ldots, A_{r-1})$. That is $S_r$ is the indices of all the suffix maxima when considering the array $[1,r-1]$. Obviously, the values of elements in $S_r$ will be a non-increasing sequence. It is natural to maintain $S_r$ as a stack to easily transition to $S_{r+1}$.

Now, for some $l \in S_r$, $(l,r)$ is a sandwich if $\text{nxt}(l)$, the element to the right of $l$ in $S_r$ satisfies $A_{\text{nxt}(l)} \leq A_r$, which is a suffix of $S_r$ since $S_r$ is non-increasing.

Furthermore, if $A_{\text{nxt}(l)} < A_r$, note the strict inequality, then $\text{nxt}(l) \notin S_{r+1}$.

So we can quickly solve the above problem by maintaining a stack of $S_r$ of $(\text{value},\text{occurance})$:

1. add the number of occurrences of values $\leq A_r$ in $S_r$ to our answer
2. then delete all values $< A_r$ in $S_r$
3. add a single occurrence of $S_r$ to the stack
4. $S_r$ has been changed to $S_{r+1}$ now

Note that we need a stack of $(\text{value},\text{occurance})$ and not only $(\text{value})$ is so that step 1 will not run in $O(n^2)$ time, if we have an array with all same values. Now, the above algorithm clearly runs in $O(n)$ time.

### Solution 1

This solution was the original solution.

We will proceed with DnC. 

Assume that for some fixed $m$, all queries satisfy $x_i \leq m < y_i$. Then we will solve this subproblem $O(n)$, so that the entire algorithm will run in $O(n \log n)$.

There are three types of queries are concerned about:

1. $x_i \leq l \leq r \leq m$
2. $m<l \leq r \leq y_i$
3. $x_i \leq l \leq m < r \leq y_i$

For types 1 and 2, use the solution presented in easy version. This can be done in $O(n)$.

Now for type $3$, we will want to consider the $l \in [1,m]$ which satisfy $A_l \geq \max(A_{l+1}, A_{l+2}, \ldots, A_m)$ and the $r \in (m,n]$ which satisfy $\max(A_{m+1}, A_{l+2}, \ldots, A_{r-1}) \leq A_r$. Define $B_l := \max(A_{l+1}, A_{l+2}, \ldots, A_m)$ and $B_r := \max(A_{m+1}, A_{l+2}, \ldots, A_{r-1})$ for convenience.

Thus, $(l,r)$ is a sandwich if it additionally satisfy $\max(B_l, B_r) \leq \min(A_l,A_r)$.

Since we are sweeping $l$ from right to left, it is helpful to consider both $A_i$ and $B_i$ to be non-decreasing here in our mental model and also note that $B_i \leq A_i$.

Let us focus on the range of $r$ is a sandwich when $l$ is fixed. We have to satisfy both $B_l \leq A_r$ and  $B_r \leq A_l$, which is a contiguous segment in $r$. So from this alone we can just do a dynamic range add and obtain $O(n \log n)$ complexity for this problem and $O(n \log^2 n)$ complexity in total. But we can do better.

We don't only have the condition that $A_i$ and $B_i$ are non-decreasing with $B_i \leq A_i$ but we also have $B_{\text{nxt}(i)} = A_i$.

For example, when $A[1,m] = [5,1,4,4,1,3,0]$, we have (in reverse order) $(A_l,B_l) = [(0,-\infty),(3,0),(4,3),(4,4),(5,4)]$.

We will study the case of:

- $(A_l,B_l) = [(x_1,-\infty),(x_2,x_1),(x_3,x_2),(x_4,x_3),\ldots]$ 
- $(A_r,B_r) = [(y_1,-\infty),(y_2,y_1),(y_3,y_2),(y_4,y_3),\ldots]$ 

These two special cases basically generalises everything. Define $x_0  = y_0 = -\infty$ for convinience.

So the ranges of $r$ that each $l$ will take is $x_{l-1} \leq y_{r}$ and $y_{r-1} \leq x_l$.

So from here, we split the $(A_r,B_r)$ into the following disjoint categories:

- $X_{l}$ is the set of all $r$ such that $x_{l-1} \leq y_r$ and $y_r < x_l$
- $Y_{l}$ is the set of all $r$ such that $y_r = x_l$

Then, observe that the set of $r$ that satisfies $x_{l-1} \leq y_{r}$ and $y_{r-1} \leq x_l$ is simply $X_l \cup Y_l \cup X_{l+1}$ and at most one extra element with $x_l = y_{r-1} < y_r$.

Therefore, if we compress the elements of $r$ into the important categories above, we only need to range add onto $O(1)$ ranges which can be done using prefix sums instead of doing more general dynamic range adds.

Implementation wise, you don't need to explicitly generate the above categories but just generate the ranges of $r$ for each element of $l$ and then "discretize" those ranges. The above proof is only showing that when you discretize those ranges, you will have need to update $O(1)$ number of ranges.

<details> <summary> Code </summary> <p>

```c++
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
#include <ext/rope>
using namespace std;
using namespace __gnu_pbds;
using namespace __gnu_cxx;
#define ll long long
#define ii pair<ll,ll>
#define iii pair<ii,ll>
#define fi first
#define se second
#define endl '\n'
#define debug(x) cout << #x << ": " << x << endl

#define pub push_back
#define pob pop_back
#define puf push_front
#define pof pop_front
#define lb lower_bound
#define ub upper_bound

#define rep(x,start,end) for(auto x=(start)-((start)>(end));x!=(end)-((start)>(end));((start)<(end)?x++:x--))
#define all(x) (x).begin(),(x).end()
#define sz(x) (int)(x).size()

#define indexed_set tree<ll,null_type,less<ll>,rb_tree_tag,tree_order_statistics_node_update>
//change less to less_equal for non distinct pbds, but erase will bug

mt19937 rng(chrono::system_clock::now().time_since_epoch().count());

int n,q;
int arr[3000005];

struct Q{
	int l,r;
	int idx;
};

vector<Q> qu;

ll ans[3000005];

ll curr=0;
vector<ii> v;
ll stkl[3000005],stkr[3000005];

ii id[3000005];

int used[3000005];
bool impt[3000005];
int block[3000005];

ll pref[3000005];
int num[3000005];

void dnc(int l,int r,vector<Q> qu){
	if (qu.empty()) return;
	int m=l+r>>1;
	
	if (l==r){
		for (auto &it:qu) ans[it.idx]=1;
		return;
	}
	
	curr=0;
	v.clear();
	rep(x,m+1,l){
		while (!v.empty() && v.back().fi<arr[x]) curr+=v.back().se,v.pob();
		if (!v.empty() && v.back().fi==arr[x]){
			if (sz(v)>=2) curr++;
			curr+=v.back().se;
			v.back().se++;
		}
		else{
			if (sz(v)>=1) curr++;
			v.pub(ii(arr[x],1));
		}
		
		curr++;
		stkl[x]=curr;
	}
	
	curr=0;
	v.clear();
	rep(x,m+1,r+1){
		while (!v.empty() && v.back().fi<arr[x]) curr+=v.back().se,v.pob();
		if (!v.empty() && v.back().fi==arr[x]){
			if (sz(v)>=2) curr++;
			curr+=v.back().se;
			v.back().se++;
		}
		else{
			if (sz(v)>=1) curr++;
			v.pub(ii(arr[x],1));
		}
		
		curr++;
		stkr[x]=curr;
	}
	
	vector<int> idx1;
	vector<int> idx2;
	
	int p=-1;
	rep(x,m+1,l){
		if (arr[x]>=p){
			idx1.pub(x);
			p=arr[x];
		}
		id[x]=ii(-1,-1);
	}
	
	p=-1;
	rep(x,m+1,r+1){
		if (arr[x]>=p){
			idx2.pub(x);
			p=arr[x];
			used[x]=sz(idx2)-1;
		}
		else{
			used[x]=-1;
		}
	}
	
	rep(x,0,sz(idx2)) impt[x]=false;
	int pl=0,pr=0;
	rep(x,0,sz(idx1)){
		while (pr<sz(idx2)-1 && arr[idx2[pr]]<=arr[idx1[x]]) pr++;
		while (pl<sz(idx2) && x && arr[idx2[pl]]<arr[idx1[x-1]]) pl++;
		
		if (pl<=pr) id[idx1[x]]=ii(pl,pr);
		impt[pl]=impt[pr+1]=true;
	}
	
	int IDX=0;
	rep(x,m+1,r+1){
		if (used[x]!=-1){
			if (impt[used[x]]) IDX++,num[IDX]=0;
			num[IDX]++;
		}
		id[x]=ii(IDX,num[IDX]);
	}
	
	vector<Q> vl,vr;
	curr=m+1;
	int mx=0;
	
	for (auto &it:qu){
		if (it.r<=m) vl.pub(it);
		else if (m<it.l) vr.pub(it);
		else{
			while (it.l<curr){
				curr--;
				
				if (id[curr]==ii(-1,-1)) continue;
				
				int bl=id[idx2[id[curr].fi]].fi,br=id[idx2[id[curr].se]].fi;
				ll psum=0;
				
				rep(x,bl,br+1){
					if (x<=mx){
						pref[x]+=num[x]+psum;
						psum+=num[x];
					}
					else{
						pref[x]=pref[x-1]+num[x];
						mx++;
					}
				}
			}
			
			ll temp=stkl[it.l]+stkr[it.r];
			int br=id[it.r].fi;
			if (mx<br) temp+=pref[mx];
			else temp+=pref[br-1]+(pref[br]-pref[br-1])/num[br]*id[it.r].se;
			
			ans[it.idx]=temp;
		}
	}
	
	dnc(l,m,vl);
	dnc(m+1,r,vr);
}

int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin.exceptions(ios::badbit | ios::failbit);
	
	cin>>n>>q;
	rep(x,1,n+1) cin>>arr[x];
	
	int a,b;
	rep(x,0,q){
		cin>>a>>b;
		qu.pub(Q({a,b,x}));
	}
	
	sort(all(qu),[](Q i,Q j){
		return i.l>j.l;
	});
	
	dnc(1,n,qu);
	
	rep(x,0,q) cout<<ans[x]<<endl;
}
```

</p></details>


### Solution 2

This solution was found by Yan Hao during testing. It is asymptotically faster at $O(n)$, but practically it is about the same speed.

The main idea is to also handle the ranges by cases, but we split by the maximum element in the range.

Indeed, suppose $m \in [x_i,y_i]$ is the index of the maximum element in the range $[x_i,y_i]$, if there are multiple maximum elements, either choose the last or the first one, choosing a middle one does not work. WLOG, we chose the first maximum element.

There are no sandwiches that satisfy $x_i \leq l < m < r \leq y_i$. Otherwise, this implies that $A_m < A_l$, which contradicts how we selected $m$.

Note that we can find the range maximum in $\langle O(n),O(1) \rangle$ using 4 Russians or something.

Let us go back to the solution of solving the problem for all queries with $(x_i,y_i) = (1,i)$. Instead of only tracking the answer, we need to track the answer for all elements in $S_r$ when we are sweeping on $r$.

In the original version, our algorithm is:

1. add the number of occurrences of values $\leq A_r$ in $S_r$ to our answer
2. then delete all values $< A_r$ in $S_r$
3. add a single occurrence of $S_r$ to the stack
4. $S_r$ has been changed to $S_{r+1}$ now

In the new version, our algorithm needs to be:

1. add the number of occurrences of values $\leq A_r$ in $S_r$ to our answer
2. then delete all values $< A_r$ in $S_r$
3. add a single occurrence of $S_r$ to the stack
4. $S_r$ has been changed to $S_{r+1}$ now
