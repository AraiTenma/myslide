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

### [aoj-0557（1年生）](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0557)の解答

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

```c++:aoj-0557.cpp
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

---

### [aoj-2331（A Way to Invite Friends）](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2331)の解答

---

### 累積和

```c++:aoj-2331.cpp
#include <iostream>
using namespace std;

const int MAX = 1e5 + 5;
int N;
int a, b;
int arr[MAX]; // 海に行くのがi(1<=i<=100001)人なら行く人の数を格納する

int main() {
  // 入力
  cin >> N;
  for (int i = 0; i < N; i++) {
    cin >> a >> b;
    arr[a]++;
    arr[b+1]--;
  }

  // 累積和をとる
  arr[1] = 1; // 海に行くのは「わたし」も入っているので「わたし」の分
  for (int i = 2; i < MAX; i++) {
    arr[i] += arr[i-1];
  }
  // iは海に行く人数を表す
  int ans;
  for (int i = 1; i < MAX; i++) {
    if (i <= arr[i]) ans = i;
  }

  // 解答（ansには海に行く最大人数が入るので解答は「わたし」の分を抜いた数）
  cout << ans - 1 << endl;
  return 0;
}
```
