# Ferries 

[Link](https://vjudge.net/contest/538106#problem/D)

## Description

There's a route composed of $s$ sections. Each section may be road or river.

At first, it's 00:00:00 time. You want to complete the route.

You can drive car to pass the roads while your velocity can't exceed $80$ km/h.

You can take ferry to cross the rivers.

For the $i$-th river, there will be $f$ ferries. You can take the $j$-th ferry at $t_j$ moment per clock. It will take you $K_i$ time to cross the $i$-th river.

For example, $t_j = 10$ then you can take the $j$-th ferry of the $i$-th river at 00:10, 01:10, 02:10 ( hr:min ) periodically.

Even though you may arrive earlier, you still have to want for the next ferry.

Calculate the minimum time needed to complete the route and the minimum possible value of the maximum velocity on each road to reach the minimum time.

## Input Format

$s > 0$

$t_j, K_i$ are given in minute.

The road length is given in km.

## Output Format

The minimum time needed in hr:min:sec format.

The minimum possible value of the maximum velocity on each road, rounded to two digit to the right of the decimal point.

## Solution

We can just drive at 80km/hr on each road to get the minimum time needed.

It's also clear that we'll complete the route earlier if driving faster.

Hence, binary search the least velocity needed.

However, there's a problem in binary search. 

The case $L=4.444\overline{999}$ and $R=4.445\overline{000}$. After rounding the former will be $4.44$ while the latter $4.45$.

It's caused by round-off error.

The proper way to do this is checking whether $L+\epsilon$ and $R+\epsilon$ are the same after rounding while $\epsilon$ is some little number.

## Time complexity

Binary search takes $O(80\times C)$ while $C$ depends on the precision.

We need $O(s+\sum f)$ to calculate the time needed.

$O(80\times C\times (s+\sum f))$ in total.

## Remark

One have to combine all the continuse roads together or there will be precision problem.

Notice that under 80km/hr, the consuming time will be integer with respect to second.

## AC code

The judge is down now so I just put my origin code without modifying the bound of binary search.

I'll replace it with new version if the judge recovers.

```cpp
#include <bits/stdc++.h>
#define debug(a) cout << #a << " = " << a << "\n";
#define F first
#define S second
using namespace std;

typedef long long ll;
ll MOD = 1000000007LL;

int n;
vector<ll> v;
vector<vector<ll>> w;

ll Calc(double x) {
	ll now = 0;
	for (int i = 0; i < n; ++ i) {
		if (w[i].empty()) {
			if (x < 1e-6) return MOD;
			now += ceil(1.0 * v[i] / x);
		} else {
			ll res = now % 3600;
			if (res > w[i].back()) {
				now = now - res + w[i].front() + 3600;
			} else {
				for (auto j : w[i]) {
					if (res <= j) {
						now = now - res + j;
						break;
					}
				}
			}
			now += v[i];
		}
	}
	return now;
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0);
	
	int t = 1;
	while (cin >> n && n) {
		v.clear();
		w.clear();
		int pre = -1;
		for (int i = 0; i < n; ++ i) {
			string a, b, c;
			int z;
			cin >> a >> b >> c >> z;
			if (c == "ferry") {
				pre = -1;
				int x;
				cin >> x;
				v.push_back(z * 60);
				w.push_back(vector<ll>(x));
				for (auto &j : w.back()) cin >> j, j *= 60;
			} else {
				if (pre == 1) v.back() += z * 3600;
				else v.push_back(z * 3600), w.push_back(vector<ll>(0));
				pre = 1;
			}
		}
		n = (int)v.size(); 
		
		ll mn = Calc(80);
		double L = 0.00;
		double R = 80.00;
		while (R - L > 1e-6) {
			double mid = (R + L) / 2.0;
			if (mn == Calc(mid)) R = mid;
			else L = mid;
		}
		if (Calc(0) == mn) R = 0;
		
		cout << "Test Case " << t << ": ";
		cout << setw(2) << setfill('0') << mn / 3600 << ":";
		cout << setw(2) << setfill('0') << mn % 3600 / 60 << ":";
		cout << setw(2) << setfill('0') << mn % 60 << " ";
		cout << fixed << setprecision(2) << R << "\n\n";
		
		t += 1;
	}
}
```