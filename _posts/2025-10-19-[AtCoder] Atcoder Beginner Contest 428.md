---
title: Atcoder Beginner Contest 428 A-E
categories: [PS/CP]
math: true
mermaid: true
pin: true
author: firework72
---

## Result

- performance : 1537
- rating : 1401 --> 1416 (+15)

## A. Grandma's Footsteps

$A$초 동안 이동하고, $B$초 동안 정지되어 있으므로 $A+B$크기의 사이클이 계속 반복된다고 생각해 보자.

$0 \leq i \lt X$를 만족하는 $i$에 대하여,

- $i$를 $A + B$로 나눈 나머지가 $A$보다 작으면, $S$만큼 이동시킨다.
- $i$를 $A + B$로 나눈 나머지가 $A$보다 크거나 같으면, 이동시키지 않는다.

각 $i$별로 순서대로 시뮬레이션 해 주면 되므로 $O(X)$의 시간 복잡도에 해결할 수 있다.

```C++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

#define FAST_IO std::cin.tie(NULL); std::cout.tie(NULL); std::ios_base::sync_with_stdio(false);
#define SIZE 500005
#define INF 987654321
#define MOD 998244353
#define FLAG 10000

int ans = 0;
int s, a, b, x;

int main() {
    FAST_IO
    cin >> s >> a >> b >> x;

    for (int i = 0; i < x; i++) {
        if (i % (a + b) < a) {
            ans += s;
        }
        else {

        }
    }

    cout << ans;
}
```

## B. Most Frequent Substrings

길이가 $K$인 모든 부분 문자열을 naive하게 구해서 등장 횟수를 `map`으로 관리해주는 문제다. 가장 많이 등장한 문자열들을 `vector`와 같은 자료구조에 담아둔 뒤 정렬해서 출력해주면 된다.

```C++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

#define FAST_IO std::cin.tie(NULL); std::cout.tie(NULL); std::ios_base::sync_with_stdio(false);
#define SIZE 500005
#define INF 987654321
#define MOD 998244353
#define FLAG 10000

int n, k;
int mx = -1;

map<string, int> m;

string s;

vector<string> ans;

int main() {
    FAST_IO
    cin >> n >> k;

    cin >> s;

    for (int i = 0; i + k - 1 < n; i++) {
        string tmp = "";
        for (int j = 0; j < k; j++) {
            tmp.push_back(s[i+j]);
        }
        m[tmp]++;
    }

    for (auto i : m) {
        if (mx < i.second) {
            mx = i.second;
            ans.clear();
            ans.push_back(i.first);
        }
        else if (mx == i.second) {
            ans.push_back(i.first);
        }
    }

    sort(ans.begin(), ans.end());

    cout << mx << '\n';

    for (string i : ans) {
        cout << i << ' ';
    }
}
```

## C. Brackets Stack Query

올바른 괄호 문자열이기 위한 조건은 아래와 같다.

1. 여는 괄호의 개수와 닫는 괄호의 개수가 같아야 한다.
2. 괄호 문자열의 접두 문자열 중에, 여는 괄호의 개수보다 닫는 괄호의 개수가 더 많은 문자열이 없어야 한다.

1번 조건은 누적 합으로 구할 수 있고, 2번 조건은 누적 최솟값(Prefix Min)으로 구할 수 있다. 이를 구현하기 위해 스택을 사용하거나 배열에서 포인터를 움직이며 값을 변경해주는 방법을 사용할 수 있다.

```C++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

#define FAST_IO std::cin.tie(NULL); std::cout.tie(NULL); std::ios_base::sync_with_stdio(false);
#define SIZE 1000005
#define INF 987654321
#define MOD 998244353
#define FLAG 10000

int ps[SIZE] = {0};
int pmn[SIZE] = {0};

int q;
int p = 0;

int main() {
    FAST_IO
    cin >> q;

    while (q--) {
        int t;
        cin >> t;
        if (t == 1) {
            char c;
            cin >> c;
            p++;
            if (c == '(') {
                ps[p] = ps[p-1] + 1;
                pmn[p] = min(pmn[p-1], ps[p]);
            }
            else if (c == ')') {
                ps[p] = ps[p-1] - 1;
                pmn[p] = min(pmn[p-1], ps[p]);
            }
        }
        else if (t == 2) {
            p--;
        }

        cout << (ps[p] == 0 && pmn[p] == 0 ? "Yes" : "No") << '\n';
    }
}
```

## D. 183184

$0 \leq k \leq 18$을 만족하는 모든 정수에 대해, 다음 과정을 수행한다.

- $l = \max(C, 10^k)$, $r = \min(C + D, 10^{k+1} - 1)$로 정의한다. 만약 $l \gt r$이라면, 이때 경우의 수는 $0$이다.
- $f(C, l)$과 $f(C, r)$ 사이의 수는 연속적이고, 이 사이에 있는 제곱수의 수는 $f(C, l)$보다 크거나 같은 제곱수 중 가장 작은 수, $f(C, r)$보다 작거나 같은 제곱수 중 가장 큰 수를 구해 주면 쉽게 구할 수 있다. 이는 이분 탐색을 이용하거나 `sqrt`함수를 사용한 뒤 오차를 보정하는 방법이 있다.

구현을 함에 있어서 Overflow에 조심해야 한다. 테스트케이스 당 시간복잡도는 $O(\log_{10}f(C, C+D))$이다.

```C++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

#define FAST_IO std::cin.tie(NULL); std::cout.tie(NULL); std::ios_base::sync_with_stdio(false);
#define SIZE 500005
#define INF 987654321
#define MOD 998244353
#define FLAG 10000

void solve() {

    ll c, d;
    cin >> c >> d;

    ll tmp = 1;
    ll ans = 0;
    int cnt = 0;

    while (tmp <= c + d && cnt <= 18) {
        if (tmp * 10 - 1 < c) {
            tmp *= 10;
            cnt++;
            continue;
        }
        ll l = max(tmp, c);
        ll r = min(tmp * 10 - 1, c + d);

        vector<ll> tmp_l, tmp_r;

        ll L = l, R = r;

        while (L > 0) {
            tmp_l.push_back(L % 10);
            L /= 10;
        }
        while (R > 0) {
            tmp_r.push_back(R % 10);
            R /= 10;
        }


        L = R = c;

        for (int i = (int)tmp_l.size() - 1; i >= 0; i--) {
            L *= 10, L += tmp_l[i];
        }
        for (int i = (int)tmp_r.size() - 1; i >= 0; i--) {
            R *= 10, R += tmp_r[i];
        }

        ll start = 0;
        ll end = 2e9;

        while (start <= end) {
            ll mid = (start + end) / 2;
            if (mid * mid < L) {
                start = mid + 1;
            }
            else {
                end = mid - 1;
            }
        }

        ll res1 = start;

        start = 0, end = 2e9;

        while (start <= end) {
            ll mid = (start + end) / 2;
            if (mid * mid > R) {
                end = mid - 1;
            }
            else {
                start = mid + 1;
            }
        }

        ll res2 = end;

        ans += res2 - res1 + 1;
        tmp *= 10;
        cnt++;
    }

    cout << ans << '\n';
}


int main() {
    FAST_IO
    int t;
    cin >> t;
    while (t--) solve();
}
```

## E. Farthest Vertex

트리의 루트 정점을 $1$번으로 설정하자. 그러면, 다음 성질이 성립한다.

- 현재 정점 $u$에서 임의의 자식 정점 $v$로 이동할 경우, $v$를 루트 정점하는 서브트리에 속한 모든 정점들까지의 거리는 $1$ 감소하고, 이 외의 정점들까지의 거리는 $1$ 증가한다.
- 현재 정점 $u$에서 부모 정점 $v$로 이동할 경우, $u$를 루트 정점으로 하는 서브트리에 속한 모든 정점들까지의 거리는 $1$ 증가하고, 이 외의 정점들까지의 거리는 $1$ 감소한다.

서브 트리에 속한 정점과 그 외의 정점들의 거리가 각각 변하므로, DFS 방문 순서대로 세그먼트 트리를 구성해 주면 lazy propagation을 이용해 한 번의 이동을 구간 쿼리를 통해 처리할 수 있다. 세그먼트 트리의 각 정점에는 `(maxDist, nodeNo)`를 저장하는 `pair`를 저장하고, 문제에서 주어진 정답을 구하기 위해서는 세그먼트 트리의 루트 정점에 담겨 있는 `nodeNo`를 추출하면 된다. (여기서 `nodeNo`는 DFS 순서가 아닌 원래 정점 번호임에 유의하자.)

구간 쿼리를 총 $2N$번 처리해야 하므로 시간 복잡도는 $O(N\log N)$이다.

```C++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

#define FAST_IO std::cin.tie(NULL); std::cout.tie(NULL); std::ios_base::sync_with_stdio(false);
#define SIZE 500005
#define INF 987654321
#define MOD 998244353
#define FLAG 10000

pair<int, int> segTree[SIZE * 4];
int n;
int s[SIZE] = {0};
int e[SIZE] = {0};
int no[SIZE] = {0};
int dist[SIZE] = {0};
int ans[SIZE] = {0};
int id = 0;

int visited[SIZE] = {0};
int lazy[SIZE * 4] = {0};

vector<int> edge[SIZE];
vector<int> childs[SIZE];

void push(int node, int start, int end) {
    if (start != end) {
        lazy[node * 2] += lazy[node];
        lazy[node * 2 + 1] += lazy[node];
    }

    segTree[node].first += lazy[node];
    lazy[node] = 0;
}

void dfs(int cur, int depth) {
    s[cur] = ++id;
    no[id] = cur;
    dist[id] = depth;
    for (int next : edge[cur]) {
        if (visited[next]) continue;
        visited[next] = 1;
        childs[cur].push_back(next);
        dfs(next, depth + 1);
    }
    e[cur] = id;
}

void makeSeg(int node, int start, int end) {
    if (start == end) {
        segTree[node].first = dist[start];
        segTree[node].second = no[start];
        return;
    }

    int mid = (start + end) / 2;
    makeSeg(node * 2, start, mid);
    makeSeg(node * 2 + 1, mid + 1, end);

    segTree[node] = max(segTree[node * 2], segTree[node * 2 + 1]);
}

void updateSeg(int node, int val, int start, int end, int left, int right) {
    push(node, start, end);
    if (start > right || end < left) {
        return;
    }

    if (left <= start && end <= right) {
        lazy[node] += val;
        push(node, start, end);
        return;
    }

    int mid = (start + end) / 2;
    updateSeg(node * 2, val, start, mid, left, right);
    updateSeg(node * 2 + 1, val, mid + 1, end, left, right);

    segTree[node] = max(segTree[node * 2], segTree[node * 2 + 1]);
}

void dfs2(int cur) {
    ans[cur] = segTree[1].second;
    for (int next : childs[cur]) {
        updateSeg(1, 1, 1, n, 1, s[next] - 1);
        updateSeg(1, -1, 1, n, s[next], e[next]);
        updateSeg(1, 1, 1, n, e[next] + 1, n);
        dfs2(next);
        updateSeg(1, -1, 1, n, 1, s[next] - 1);
        updateSeg(1, 1, 1, n, s[next], e[next]);
        updateSeg(1, -1, 1, n, e[next] + 1, n);
    }
}

int main() {
    FAST_IO
    cin >> n;

    for (int i = 0; i < n - 1; i++) {
        int a, b;
        cin >> a >> b;
        edge[a].push_back(b);
        edge[b].push_back(a);
    }

    visited[1] = 1;
    dfs(1, 0);
    makeSeg(1, 1, n);
    dfs2(1);

    for (int i = 1; i <= n; i++) {
        cout << ans[i] << '\n';
    }
}
```
