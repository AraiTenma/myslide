### こんにちは！

```c++:Hello.cpp
#include <iostream>
using namespace std;

int main() {
  cout << "Hello, World!" << endl;
  
  return 0;
}
```

---

### aoj-0557（1年生）の解説

---

### 愚直な解法（再帰で）

```c++:aoj-0557_rec.cpp
#include <iostream>
using namespace std;

typedef long long ll;
int N;
int a[110];
ll ans = 0;

// n：何番目の数まで使えるか、sum：今の合計値
void solve(int n, int sum) {
  if (n >= N) return;
  if (n == N - 1) ans += (sum == a[n]);

  if (sum + a[n] <= 20) solve(n + 1, sum + a[n]);
  if (sum - a[n] >= 0) solve(n + 1, sum - a[n]);
}

int main() {
  // 入力
  cin >> N;
  for (int i = 0; i < N; i++) cin >> a[i];

  // 再帰で全探索
  solve(0, 0);
  cout << ans << endl;
  return 0;
}
```

---

### メモ化再帰

```c++:aoj-0557_memo_rec.cpp
#include <iostream>
using namespace std;

typedef long long ll;
int N;
int a[110];
ll memo[110][21];

// n：何番目の数まで使えるか、sum：今の合計値
ll solve(int n, int sum) {
  if (memo[n][sum]) return memo[n][sum];

  if (n == N - 1) return (sum == a[n]);

  if (sum + a[n] <= 20) memo[n][sum] += solve(n + 1, sum + a[n]);
  if (sum - a[n] >= 0) memo[n][sum] += solve(n + 1, sum - a[n]);
  return memo[n][sum];
}

int main() {
  // 入力
  cin >> N;
  for (int i = 0; i < N; i++) cin >> a[i];

  // メモ化再帰
  cout << solve(1, a[0]) << endl;
  return 0;
}
```

---

### 動的計画法

```
#include <iostream>
using namespace std;

typedef long long ll;

int main() {
  int N;
  cin >> N;
  int a[N];
  for (int i = 0; i < N; i++) cin >> a[i];

  ll dp[N+1][21];
  fill(dp[0], dp[N], 0);

  dp[0][a[0]] = 1;
  for (int i = 1; i < N - 1; i++) {
    for (int j = 0; j <= 20; j++) {
      if (dp[i-1][j] > 0) {
        if (j + a[i] <= 20) dp[i][j + a[i]] += dp[i-1][j];
        if (j - a[i] >= 0) dp[i][j - a[i]] += dp[i-1][j];
      }
    }
  }

  // 解答
  cout << dp[N-2][a[N-1]] << endl;
  return 0;
}
```

