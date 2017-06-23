### こんにちは！

```
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

```
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

