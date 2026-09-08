# C++

## Template

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ms(s, n) memset(s, n, sizeof(s))
#define all(a) a.begin(), a.end()
#define sz(a) int((a).size())
#define in(t, x) (t.find(x) != t.end())
#define FOR(i, a, b) for(int i = (a); i <= (b); i++)
#define FORd(i, a, b) for(int i = (a) - 1; i >= b; i--)
#define pb push_back
#define pf push_front
#define fi first
#define se second
#define mp make_pair
#define endl "\n"

typedef long long ll;
typedef unsigned long long ull;
typedef long double ld;
typedef pair<int, int> pi;
typedef vector<int> vi;
typedef vector<pi> vii;
typedef vector<pair<pair<int, int>, int>> vpii;

const int MOD = (int) 1e9 + 7;
const int INF = (int) 1e9 + 2804;
const int MAXN = (int) 1e5 + 105;
inline ll gcd(ll a, ll b){ll r; while(b){r=a%b; a=b; b=r;} return a;}
inline ll lcm(ll a, ll b){return a/gcd(a,b)*b;}

int main(){
    #ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    #endif 
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); cout.tie(nullptr);

}
```

## Kỹ thuật dùng Mảng

### Một chiều

#### Prefix sum

Tính tổng từ l --> k : dp[r] - d[l - 1] / dp[r] nếu l = 0

```cpp
int n; cin >> n;
int a[n]; 
ll dp[n];
for(int i = 0; i < n; i++){
    cin >> a[i];
    if(i == 0) dp[i] = a[i];
    else dp[i] = dp[i - 1] + a[i];
}
int q; cin >> q;
while(q--){
    int l, r; 
    cin >> l >> r; 
    --l; --r; // nếu chỉ số l bắt đầu tính từ 1
    if(l == 0) cout << dp[r];
    else cout << dp[r] - dp[l - 1];
    }
```

#### Difference array

- Update phần tử l->r lên k đơn vị : d[l] += k và d[r + 1] -= k
- Tính mảng cộng dồn của mảng hiệu -> quay lại mảng gốc ban đầu

```cpp
ll d[n + 5]; // tránh bị lỗi khi đó là chỉ số cuối cùng
for(int i = 0; i < n; i++){
    if(i == 0) d[i] = a[i];
    else d[i] = a[i - 1] - a[i]
}
int q; cin >> q;
while(q--){
    int l, r, k; 
    cin >> l >> r >> k; 
    d[l] += k;
    d[r + 1] -= k;
}
for(int i = 0; i < n; i++){
    if(i == 0) dp[i] = d[i];
    else dp[i] = dp[i - 1] + d[i];
    cout << dp[i] << " ";
}
```

#### Sliding window

```cpp
ll sum = 0;
for(int i = 0; i < k; i++){
    sum += a[i];
}
ll res = sum, pos = 0;
for(int i = 1; i <= n - k; i++){
    sum = sum - a[i - 1] + a[i + k - 1];
    if(sum > res){ //nếu có dấu bằng là cập nhật dãy cuối cùng, còn ko có là dãy đầu 
        res = sum;
        pos = i;
    }
}
cout << res << endl;
for(int i = 0; i < k; i++) cout << a[pos + i] << " ";
```

#### Two pointer

- In số phần tử ít hơn của mỗi phần tử ở dãy 1 so với dãy 2
  ```cpp
  int n, m; cin >> n >> m;
  int a[n], b[m];
  for(int &x : a) cin >> x;
  for(int &x : b) cin >> x;
  int i = 0, j = 0;
  while(i < n && j < m){
      if(a[i] < b[j]) i++;
      else{
          cout << i << " ";
          j++;
      }
  }
  while(j < m){
      cout << n << ' ';
      j++;
  }

  //C2
  int j = 0;
  for(int i = 0; i < m; i++){
      while(j < n && a[j] < b[i]) j++;
      cout << j << ' '; 
  }
  ```
- Tìm số cặp của các phần tử giống nhau ở 2 dãy
  ```cpp
  int n, m; cin >> n >> m;
  int a[n], b[m];
  for(int &x : a) cin >> x;
  for(int &x : b) cin >> x;
  int i = 0, j = 0;
  ll cnt = 0;
  while(i < n && j < m){
      if(a[i] == b[j]){
          int x = a[i], d1 = 0, d2 = 0;
          while(a[i] == x){
              d1++;
              i++;
          }
          while(b[j] == x){
              d2++;
              j++;
          }
          cnt += 1ll * d1 * d2;
      }
      else if(a[i] < b[j]) i++;
      else j++;
  }
  cout << cnt << endl;
  ```
- In ra vị trí 2 phần tử có tổng bằng k
  ```cpp
  int n, k; cin >> n >> k;
  int a[n];
  for(int &x : a) cin >> x;
  int i = 0, j = n - 1;
  while(i <= j){
      ll sum = a[i] + a[j];
      if(sum == k){
          cout << i + 1 << " " << j + 1 << endl;
          return 0;
      }
      else if(sum < k) i++;
      else j--;
  }
  cout << "IMPOSSIBLE\n";
  ```
- Đêm các cặp phần tử có tổng bằng k
  ```cpp
  int n, k; cin >> n >> k;
  int a[n];
  for(int &x : a) cin >> x;
  int i = 0, j = n - 1;
  ll cnt = 0;
  while(i <= j){
      ll sum = a[i] + a[j];
      if(sum == k){
          int x1 = a[i], x2 = a[j], d1 = 0, d2 = 0;
          while(a[i] == x1){
              d1++; i++;
          }
          while(a[j] == x2){
              d2++; j--;
          }
          if(x1 == x2) cnt += 1ll * d1 * (d1 - 1) / 2;
          else cnt += 1ll * d1 * d2;
      }
      else if(sum < k) i++;
      else j--;
  }
  cout << cnt << endl;
  ```
- Đêm 3 phần tử có tổng bằng k
  ```cpp
  int n, k; cin >> n >> s;
  int a[n];
  for(int &x : a) cin >> x;
  ll cnt = 0;
  for(int i = 0; i < n; i++){
      int l = i + 1, r = n - 1;
      int k = s - a[i];
      while(l < r){
          ll sum = a[l] + a[r];
          if(sum == k){
              int x1 = a[l], x2 = a[r], d1 = 0, d2 = 0; 
              while(a[l] == x1){
                  d1++; l++;
              }
              while(a[r] == x2){
                  d2++; r--;
              }
              if(x1 == x2) cnt += 1ll * d1 * (d1 - 1) / 2;
              else cnt += 1ll * d1 * d2;
          }
          else if(sum < k) l++;
          else r--;
      }
  }
  cout << cnt << endl;
  ```

### Hai chiều

#### Kỹ thuật duyệt các ô liền kề

- Duyệt ô liền kề -> dùng 2 mảng hoặc 1 pair để lưu lượng thay đổi khi di chuyển sang các ô ở cột và hàng khác nhau

  - move[4] -> duyệt 4 ô xung quanh (chung cạnh)

  ```cpp
  int dx[4] = {-1, 0, 0, 1};
  int dy[4] = {0, -1, 1, 0};
  pair<int, int> move4[4] = {{-1, 0}, {0, -1}, {1, 0}, {0, 1}};
  ```

  - move[8] -> duyệt 8 ô xung quanh (chung đỉnh)

  ```cpp
  int dx[8] = {-1, -1, -1, 0, 0, 1, 1, 1};
  int dy[8] = {-1, 0, 1, -1, 1, -1, 0, 1};
  pair<int, int> move8[8] = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
  ```

  - knightmove[8] -> duyệt 8 ô là nước đi của con mã

  ```cpp
  int dx[8] = {-2, -2, -1, -1, +1, +1, +2, +2};
  int dy[8] = {-1, +1, -2, +2, -2, +2, -1, +1};
  pair<int, int> knigntMove[8] = {{-2, -1}, {-2, 1}, {-1, -2}, {-1, 2}, {1, -2}, {1, 2}, {2, -1}, {2, 1}};
  ```
- Ứng dụng :

  ```cpp
  for(int i = 1; i < n; i++){
      for(int j = 1; j < n; j++){
          ll sum = a[i][j];
          for(int k = 0; k < 8; k++){
              int i1 = i + dx[k];
              int j1 = j + dy[k];
              sum += a[i1][j1];
          }
          res = max(res, sum); 
      }
  }
  cout << res << endl;
  ```

#### Prefix sum

```cpp
pre[n + 5][n + 5];
for(int i = 1; i <= n; i++){
    for(int j = 1; j <= m; j++){
        pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + a[i][j];
    }
}
```

#### Kỹ thuật loang

```cpp
//Loang
void spread(int i, int j){
    a[i][j] = 0; //đánh dấu đã thăm
    for(int k = 0; k < 4; k++){
        int i1 = i + dx[k];
        int j1 = j + dy[k];
        if(i1 >= 0 && i1 < n && j1 >= 0 && j1 < m && a[i1][j1] == 1){
            spread(i1, j1);
        }
    }
}
int main(){
    //Count island => Kĩ thuật Loang
    int cnt = 0;
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(a[i][j] == 1){
                cnt++;
                spread(i, j);
            }
        }
    }
    cout << cnt << endl;
}
```

## Bit manipulation

### Bitwise

- Biểu diễn nhị phân : dùng unsigned int vì không có dấu xét bit cho dễ
- Ứng dụng phép AND : `b & 1 = b % 2 == 1` vì khi AND 1 số bất kì với 1 thì kết quả chỉ bằng 1 khi số đó có bit cuối là 1 ⇒ số lẻ
- Ứng dụng phép XOR : Áp dụng cho 1 mảng chỉ có duy nhất 1 phần tử có số lần xuất hiện là lẻ. Tìm phần tử đó ⇒ vì các số giống nhau XOR với nhau đều bằng 0
  ```cpp
  //Cho 1 mảng chỉ có duy nhất 1 pt có số lần xuất hiện là lẻ
      int n; cin >> n;
      int a[n];
      for(int i = 0; i < n; i++) cin >> a[i];
      int res = 0;
      for(int x : a) res ^= x;
      cout << res;
  ```
- Dịch bit :
  - **left shift <<** : dịch trái k bit = nhân 2^k.  VD : 1 << k → 1 * 2^k
  - **right shift >>** : dịch phải k bit = chia 2^k. VD : 1 >> k -> 1 / 2^k

### Bitmasks

- Ứng dụng bitmasks : Sinh tập con có n phần tử :
  ```cpp
  //Sinh ra tập con có n phần tử (N*2^N -> như giải thuật sinh)
  void binGenerate(int a[], int n){
      for(int i = 0; i < (1 << n); i++){ //Duyệt các số i từ 0 -> 2^n - 1
          //Dựa vào cấu hình nhị phân của i để suy ra cấu hình 1 tập con tương ứng
          // 0 = 000 : {∅}
          // 1 = 001 : {9} ...
          for(int j = 0; j < n; j++){ //Duyệt các bit từ 0 -> n - 1 (cấu hình i)
              if(i & (1 << j)){ // Kiếm tra thử bit thứ j của i được bật là 1 ko bằng cách i & 2^j
                  cout << a[j] << ' ';
              }
          }
          cout << endl;
      }
  }
  ```

## Number theory

### Kiểm tra số nguyên tố

```cpp
bool prime(ll n){
	if(n < 2) return false;
	for(int i = 2; i <= sqrt(n); i++){
		if(n % i == 0) return false;
	}
	return true;
}
```

### Phân tích thừa số nguyên tố

```cpp
ll factorize(ll n){
    for(int i = 2; i <= sqrt(n); i++){
	    //ước đầu tiên khác 1 mà n chia hết => số nguyên tố nhỏ nhất
        if(n % i == 0) return i;
    }
}
// in theo format 
void factorize2(ll n){
    if(n == 1){
        cout << 0 << " " << 1 << endl;
        return;
    }
    else{
        int total = 1;
        for(int i = 2; i <= sqrt(n); i++){
            int cnt = 0;
            while(n % i == 0){
                cnt++;
                n /= i;
            }
            cout << i << "^" << cnt;
            if(n > 1) cout << " * ";
            total *= cnt + 1;
        }
        if(n > 1) cout << n << "^1";
        cout << " " << total << endl;
    }
}

// in theo format
void factorize3(ll n){
    for(int i = 2; i <= sqrt(n); i++){
        if(n % i == 0){
            int cnt = 0;
            while(n % i == 0){
                cnt++;
                n /= i;
            }
            if(cnt > 1) cout << i << "^" << cnt << " ";
            else cout << i << " ";
        }
    }
    if(n > 1){
        cout << n << endl;
    }
}
```

### Sàng số nguyên tố

- Kiểm tra nhiều lần trong 1 khoảng n<=10^7

  ```cpp
  //Để sàng đc <= n số -> tạo mảng cỡ n + 1 phần tử
  const int k = 100000; 
  bool nt[k + 1];
  void sang(){
      //cho tất cả các giá trị bằng true
      for(int i = 0; i <= k; i++){
          nt[i] = true;
      }
      //cho số 0 và 1 false
      nt[0] = nt[1] = false;

      //kt số nào là nguyên tố
      for(int i = 2; i <= sqrt(k); i++){
          if(nt[i]){
              //loại tất cả các bội < n của nó
              for(int j = i * i; j <= k; j += i){ 
              // j phải bắt đầu từ bình phương của số nguyên tố sau đó tăng bước nhảy lên i lần
                  nt[j] = false;
              }
          }
      }
  }
  //code gọn hơn
  const int MAXN = 1e7 + 9;
  bool prime[MAXN];
  void sieve(){
      memset(prime, true, sizeof(prime));
      prime[0] = prime[1] = false;
      for(int i = 2; i <= sqrt(MAXN); i++){
          if(prime[i]){
              for(int j = i * i; j <= MAXN; j += i){
                  prime[j] = false;
              }
          }
      }
  }
  ```
- Ứng dụng sàng số nguyên tố :

  - Sàng số nguyên tố trong đoạn [a,b]

  ```cpp
  void sang(ll l, ll r){
      int p[r - l + 1];   
      for(ll i = 0; i <= r; i++) p[i] = 1;
      for(ll i = 2; i <= sqrt(r); i++){
          // bội nhỏ nhất >= L của i : L + i - 1 / i * i
          for(ll j = max(i * i, (l + i - 1) / i * i); j <= r; j += i){
              // lưu và kiểm tra chỉ số j - l => dịch chuyển chỉ số của phần tử ra
              p[j - l] = 0;

          }
      }
      //duyệt
      for(ll i = max(2ll, l); i <= r; i++){
          if(p[i - l]) cout << i << " ";
      }
  }
  ```

  - Kiểm tra số đẹp trong đoạn [a,b]

  ```cpp
  const int MAXN = 1e7 + 1;
  int nt[MAXN], dep[MAXN];
  void sang(){
      memset(nt, 1, sizeof(nt));
      nt[0] = nt[1] = 0;
      for(int i = 2; i <= sqrt(MAXN); i++){
          if(nt[i]){
              //i là số nguyên tố
              //số đẹp là bội của nguyên tố và chia hết cho bình phương nguyên tố đó
              int tmp = i * i;
              //k là bội của tmp
              for(int k = 1; k * tmp <= MAXN; k++){
                  dep[k * tmp] = 1;
              }
              for(int j = i * i; j <= MAXN; j += i){
                  nt[j] = 0;
              }
          }
      }
  }
  void check(int a, int b){
      sang();
      for(int i = a; i <= b; i++){
          if(dep[i]) cout << i << ' ';
      }
  }
  ```

  - Sinh ra các số hoàn hảo nhanh

  ```cpp
  // nếu p là số nt thì 2^p - 1 cũng là số nt => (2^p-1)*(2^(p-1)) là số hh
  bool nt(int n){
      for(int i = 2; i <= sqrt(n); i++){
          if(n % i == 0) return false;
      }
      return n > 1;
  }
  ll hh[10];
  int cnt = 0;
  void perfect(){
      for(int p = 2; p <= 32; p++){
          if(nt(p)){
              int tmp = (int)pow(2, p) - 1;
              if(nt(tmp)){
                  hh[cnt] = 1ll * (int)pow(2, p - 1) * tmp;
                  ++cnt;
              }
          }
      }
  }
  ```

  - Sàng phi hàm Euler

  ```cpp
  int phi[1000001];
  void init(){
      for(int i = 1; i <= 1000000; i++) phi[i] = i;
      for(int i = 2; i <= 1000000; i++){
          if(phi[i] == i){ //i là số nt
              phi[i] = i - 1; // các số nt <= n
              for(int j = i * 2; j <= 1000000; j += i){
                  phi[j] = phi[j] - phi[j] / i;
              }
          }
      }
  }
  ```

  - Sàng ước số nguyên tố nhỏ nhất

  ```cpp
  int prime[1000001];
  void sang(){
      for(int i = 1; i <= 1000000; i++) prime[i] = i; // coi mỗi số là 1 ước SNT nhỏ nhất của chính nó
      for(int i = 2; i <= sqrt(1000000); i++){
          if(prime[i] == i){ // i là SNT
              for(int j = i * i; j <= 1000000; j += i){ // duyệt bội của i
                  if(prime[j] == j){//tránh bị trùng vd như 12 = 2^2 * 3 -> cập nhập số 2 ko cập nhập só 3
                      prime[j] = i;// ước NT nhỏ nhất của j là i
                  }
              }
          }
      }
  }
  ```

### Kiểm tra số hoàn hảo

- Trong miền long long có 8 số hoàn hảo

```cpp
bool perfect(ll n){
    ll s = 1;
    for (int i = 2; i <= sqrt(n); i++){
        if (n % i == 0){
            s += i;
            if (i != n / i) s += n / i;
        }
    }
    return s == n;
}
```

### Kiểm tra số đối xứng

```cpp
bool palindrome(ll n){
	if(n < 10) return true;
	int temp = 0, res = n;
	while(n != 0){
		temp = temp * 10 + n % 10;
		n /= 10;
	}
	if(temp == res) return true;
	else return false;
}
```

### Kiểm tra số chính phương

```cpp
bool square(ll n){
	ll c = sqrt(n);
	if(n == c * c) return true;
	else return false;
}
```

### Số Fibonacci

- Chỉ có 93 số đầu là lưu được với giá trị long long
- Đệ quy :
  ```cpp
  ll fibo(int n){ //0 1 1 2 3 5 8 ...
      if(n == 1 || n == 0) return n;
      else return fibo(n - 1) + fibo(n - 2); 
  }
  // Cách khác
  ll fi(int n){ // 1 1 2 3 5 8 13 ...
      if(n == 1) return 0;
      if(n == 2) return 1;
      return fi(n - 1) + fi(n - 2);
  }
  ```
- Khởi tạo mảng 93 số Fibo :
  ```cpp
  ll f[100]; 
  void fibo(){
      f[0] = 0;
      f[1] = 1;
      for(int i = 2; i <= 92; i++){
          f[i] = f[i - 1] + f[i - 2];
      }
      //for(int i = 0; i <= 92; i++) cout << f[i]<< ' ';
  }
  ```
- Kiểm tra số fibonacci -> sinh 93 số đầu -> kt số đã cho có trong mảng fibo[n] không
  ```cpp
  bool checkFibo1(ll n){
      for(int i = 0; i <= 92; i++){
          if(n == f[i]) return true;
      }
      return false;
  }
  ```
- Kiểm tra số Fibonacci không dùng mảng
  ```cpp
  bool checkFibo2(ll n){
      if(n == 1 || n == 0) return true;
      ll f1 = 0, f2 = 1;
      for(int i = 0; i <= 92; i++){
          ll fn = f1 + f2;
          if(fn == n) return true;
          f1 = f2;
          f2 = fn;
      }
      return false;
  }
  ```

### UCLN và BCNN

- UCLN Đệ quy :

  ```cpp
  ll gcd(ll a, ll b){
      if(b == 0) return a;
      else return gcd(b, a % b);
  }
  ```
- UCLN Không đệ quy :

  ```cpp
  ll gcd2(ll a, ll b){
      ll r;
      while(b != 0){
          r = a % b;
          a = b;
          b = r;
      }
      return a;
  }
  ```
- BCNN :

  ```cpp
  ll lcm(ll a, ll b){
      return a / gcd(a, b) * b; //tránh bị tràn số
  }
  ```
- Hàm có sẵn trong C++ :

  - `__gcd(a, b);`
  - `__lcm(a, b);`
- Ứng dung:

  - GCD và LCM của các phần tử trong mảng

  ```cpp
  void GCDandLCM(ll a[], int n){
      ll res = 0, ans = 1;
      for(int i = 0; i < n; i++){
          res = gcd(res, a[i]);
          ans = lcm(ans, a[i]);
      }
      cout << res << endl << ans << endl;
  }
  ```

  - Tính số nguyên tố cùng nhau -> 2 số có ucln là 1

  ```cpp
  bool co_prime(int a, int b){ 
      if(gcd(a, b) == 1) return true;
      else return false;
  }
  ```

### Phi hàm Euler:

```cpp
ll phi(ll n){
	ll res = n;
	for(int i = 2; i <= sqrt(n); i++){
		if(n % i == 0){ // i là thừa số nguyên tố của n
			res -= res / i; //(n - n/p)
		    while(n % i == 0) n /= i; // loại bỏ những thừa số nguyên tố giống nó
		}
	}
	if(n > 1) res -= res / n; //thừa số nguyên tố cuối cùng
	return res;
}
```

### Tổ hợp

- Đệ quy :
  ```cpp
  ll C(int n, int k){ // nCk CT truy hồi: C(n, k)=C(n - 1, k - 1) + C(n - 1, k)
      if(k == 0 || k == n) return 1;
      return C(n - 1, k - 1) + C(n - 1, k);
  }
  ```
- Ứng dụng CT: nCk = nC(n - k) = n*(n-1)*(n-2)*...*(n-k+1)/1*2*3*...*k
  ```cpp
  ll toHop(ll n, ll k){ 
      ll res = 1;
      k = min(k, n - k);
      for(int i = 1; i <= k; i++){
          res *= (n - i + 1);
          res /= i;
      }
      /*
      ll res = 1;
      k = min(k, n - k);
      for(int i = 0; i < k; i++){
          res *= (n - i);
          res /= (i + 1);
      }
      */
      return res;
  }
  ```
- Dùng Quy hoạch động -> số lớn được
  ```cpp
  ll toHop2(ll n, ll k){
      //C[i][j] tổ hợp chập j của i 
      ll C[n + 1][k + 1];
      for(int i = 0; i <= n; i++){//hàng
          for(int j = 0; j <= i; j++){//cột
              if(j == 0 || j == i) C[i][j] = 1;
              else C[i][j] = C[i - 1][j] + C[i - 1][j - 1];
          }
      }
  }
  ```

### Lũy thừa nhị phân:

- Không đệ quy :
  ```cpp
  ll binPow(ll a, ll b){
      ll res = 1;
      while(b){
          if(b % 2 == 1) res *= a;
          //lũy thừa của bit xét lên
          a *= a;
          //loại bỏ bit đó đi
          b /= 2;
      }
      return res; //Tính lũy thừa thì dùng hàm này ko cần dùng hàm khác
  }
  // chia dư
  ll powMOD(ll a, ll b){
      ll res = 1;
      while(b){
          if(b % 2 == 1){
              res = ((res % MOD) * (a % MOD)) % MOD;
          }
          a = ((a % MOD) * (a % MOD)) % MOD;
          b /= 2;
      }
      return res; 
  }
  ```
- Đệ quy :
  ```cpp
  ll du(ll a, ll b){
      return ((a % MOD) * (b % MOD)) % MOD;
  }

  ll binpow(ll a, ll b){ // Lũy thừa nhị phân
      if(b == 0) return 1;
      ll x = binpow(a, b / 2);
      if(b % 2 == 0) return du(x, x); // a^b/2 * a^b/2
      else{
          ll res = du(x, x);
          return du(res, a); // a^b/2 * a^b/2 * a;
      }
  }
  ```

### Giai thừa chia dư

- Không đệ quy :
  ```cpp
  ll gtMOD(int n){
      ll res = 1;
      for(int i = 1; i <= n; i++){
          res = ((res % MOD) * (i % MOD)) % MOD;
      }
      return res;
  }
  ```
- Đệ quy :
  ```cpp
  ll gt(int n){
      if(n == 0) return 1;
      else return ((n % MOD) * (gt(n - 1) % MOD)) % MOD;
  }
  ```

### Công thức Legendre

```cpp
// Ứng dụng 
// Tính số ước d(n) = (e1 + 1)(e2 + 1)...(ek + 1) ek là bậc thừa số nguyên tố
// Tính tích của các ước của n p(n) = n ^(d(n) / 2) 
int degree(int n, int k){
    int res = 0;
    for(int i = k; i <= n; i *= k){
        res += n / i;
    }
    return res;
}
```

### Đồng dư (Modular Arithmetic)

- Công thức cơ bản :

  - (*a* − *b*) % *c* =[(*a* % *c*)−(*b* % *c*)+*c*] % *c*
  - (*a* + *b*) % *c* =[(*a* % *c*)+(*b* % *c*)] % *c*.
  - (*a* × *b*) % *c* =[(*a* % *c*)×(*b* % *c*)] % *c*.
  - *(a^m)* % *c* =[(*a* % *c*)^m] % *c*.
- Công thức nâng cao :

  - *(a / b) % c = [(a % c) x (b^-1 % c)] % c*
  - **b^-1** : nghịch đảo modulo → giải thuật Euclid mở rộng + nhỏ Fermat
  - Cài đặt Euclid :

  ```cpp
  int extended_gcd(int a, int b, int& x, int& y){
      if(b == 0){
          x = 1;
          y = 0;
          return a;
      }
      int x1, y1;
      int d = extended_gcd(b, a % b, x1, y1);
      //công thức
      x = y1;
      y = x1 - y1 * (a / b);
      return d;
  }
  int euclid_inverse(int a, int m){
      int x, y;
      int d = extended_gcd(a, m, x, y);
      if(d != 1){
          return -1;
      }
      else{
  ```

## Recursion (Đệ quy)

### Đổi sang hệ nhị phân

```cpp
void binConvert(ll n){
	if(n == 0) return;
	convert(n / 2);
	cout << n % 2;
}

int main(){
	ll n; cin >> n;
	if(n == 0) cout << 0 << endl;
	convert(n);
}
```

### Đổi sang hệ hexadecimal

```cpp
void hexConvert(ll n){
	if(n == 0) return;
	convert(n / 16);
	int r = n % 16; // r = (0, 15)
	if(r < 10) cout << r;
	else cout << (char)(r + 55); // A B C D ... = 65 66 67 68 ...
}
```

### Dãy số

#### In từ trái sang phải

```cpp
void inphai(ll n){
	if(n < 10){
		cout << n << ' '; 
        return;
	}
	inphai(n / 10);
	cout << n % 10 << " ";
}
```

#### In từ phải sang trái

```cpp
void intrai(ll n){
	if(n < 10){
		cout << n << ' '; 
        return;
	}
	cout << n % 10 << " ";
	intrai(n / 10);
}
```

#### Tổng chữ số

- Đệ quy :
  ```cpp
  ll sum_digit(ll n){
  	if(n < 10) return n;
  	else return n % 10 + sum_digit(n / 10);
  }
  ```
- Không đệ quy :
  ```cpp
  ll sum = 0;
  int digit(ll n){
  	int dem = 0;
  	if(n == 0) return 1;
  	while(n){
  		dem++;
  		sum += n % 10;
  		n /= 10;
  	}
  	return dem;
  }
  ```

#### Đếm chữ số

```cpp
ll count_digit(ll n){
	if(n < 10) return 1;
	else return 1 + count_digit(n / 10);
} 
```

#### Chữ số đầu tiền trong dãy số

```cpp
ll first_num(ll n){
	if(n < 10) return n;
	else return first_num(n / 10);
}
```

#### Chữ số lớn nhất

```cpp
ll max_num(ll n){
	if(n < 10) return n;
	else return max(n % 10, max_num(n / 10));
}
// Cách khác
void fmax(ll n){
 	if(n < 10) cout << n;
 	int max1 = 0;
 	if(n % 10 > max1){
 		max1 = n % 10;
 		fmax(n / 10);
 	}
 	cout << max1 << endl;
}
```

#### Chữ số nhỏ nhất

```cpp
ll min_num(ll n){
	if(n < 10) return n;
	else return min(n % 10, min_num(n / 10));
}
```

#### Tổng chữ số chẵn

```cpp
ll even_sum(ll n){
	if(n < 10){
		if(n % 2 == 0) return n;
		else return 0;
	}
	else{
		if(n % 10 % 2 == 0) return n % 10 + even_sum(n / 10);
		else return even_sum(n / 10);
	}
}
```

#### Tổng chữ số lẻ

```cpp
ll odd_sum(ll n){
	if(n < 10){
		if(n % 2 != 0) return n;
		else return 0;
	}
	else{
		if(n % 10 % 2 != 0) return n % 10 + odd_sum(n / 10);
		else return odd_sum(n / 10);
	}
}
```

#### Số toàn chẵn

- Không đệ quy :
  ```cpp
  bool evenCheck(int n){
  	while(n != 0){
  		if(n % 2 != 0) return false;
  		else n /= 10;
  	}
  	return true;
  }
  ```
- Đệ quy :
  ```cpp
  bool even_check(ll n){
  	if(n < 10){
  		if(n % 2 == 0) return true;
  		else return false;
  	}
  	else{
  		if(n % 10 % 2 != 0) return false;
  		return even_check(n / 10);
  	}
  }
  ```

#### Số toàn lẻ

- Không đệ quy :
  ```cpp
  bool oddCheck(int n){
  	while(n != 0){
  		if(n % 2 == 0) return false;
  		else n /= 10;
  	}
  	return true;
  }
  ```
- Đệ quy :
  ```cpp
  bool odd_check(ll n){
  	if(n < 10){
  		if(n % 2 == 0) return false;
  		else return true;
  	}
  	else{
  		if(n % 10 % 2 == 0) return false;
  		return odd_check(n / 10);
  	}
  }
  ```

#### Các bài toán tính tổng chữ số và công thức rút gọn

```cpp
// S = 1+2+3+..+n --> S = n(n + 1)/2
ll sum1(int n){
	if(n == 1) return 1;
	else return n + sum1(n - 1);
}

// S = 1^2+2^2+3^2+4^2+..+n^2 --> S = n(n + 1)(2n + 1)/6
ll sum2(int n){
	if(n == 1) return 1;
	else return n * n + sum2(n - 1);
}

//S = 1^3+2^3+3^3+..+n^3 --> S = (n * (n + 1)/2)^2
ll sum3(int n){
	if(n == 1) return 1;
	else return n * n * n + sum3(n - 1);
}

//S=-1+2-3+4-5+6+..+(-1)^n * n --> S = n/2 (n chẵn) = (n - 1)/2 - n (n lẻ)
ll sum4(int n){
	if(n == 1) return -1;
	if(n % 2 == 0) return n + sum4(n - 1);
	else return -n + sum4(n - 1);
}

//S = 1/1 + 1/2 + 1/3 + ... + 1/n
double sum5(int n){
	if(n == 1) return 1;
	return 1.0 / n + sum5(n - 1);
}

//Tổng chia hết cho 3 có thể dùng CT 3ll*m(m+1)/2 vs m=n
ll sum6(int n){
	ll s = 0;
	for(int i = 3; i <= n; i += 3){
      s += i;
   }
}
```

#### Kiểm tra số tăng dần (luôn tăng)

```cpp
bool increase(int n){
    int r = n % 10;
    while(n != 0){
        n /= 10;
        if(n % 10 > r) return false;
        r = n % 10;
    }
    return true;
}
```

#### Kiểm tra số giảm dần (luôn giảm)

```cpp
bool decrease(int n){
    int r = n % 10;
    while(n != 0){
        n /= 10;
        if(n % 10 < r) return false;
        r = n % 10;
    }
    return true;
}
```

### Mảng

#### Mảng đối xứng

```cpp
bool dx(int a[], int l, int r){
	if(l > r) return true;
	if(a[l] != a[r]) return false;
	return dx(a, l + 1, r - 1);
}
```

#### In từ trái qua bên phải mảng

```cpp
void intrai(int a[], int n){
	//Cách 1
	if(n == 1){
		cout << a[0] << " ";
		return;
	}
	//Cách 2:
	//if(n == 0) return;
	intrai(a, n - 1);
	cout << a[n - 1] << ' ';
}
```

#### In từ phải sang trái mảng

```cpp
void in2(int a[], int n){
	// if(n == 1){
	// 	cout << a[0] << " ";
	// 	return;
	// }
	if(n == 0) return;
	cout << a[n - 1] << ' ';
	in2(a, n - 1);
}
```

#### Kiểm tra mảng toàn phần tử chẵn

```cpp
bool check(int a[], int n){
	if(n == 1){
		if(a[0] % 2 != 0) return false;
		else return true;
	}
	else{
		if(a[n - 1] % 2 != 0) return false;
		else return check(a, n - 1);
	}
}
```

#### Kiểm tra mảng luôn tăng dần

```cpp
bool increase_check(int a[], int n){
	if(n == 1) return true;
	if(a[n - 1] <= a[n - 2]) return false;
	return increase_check(a, n - 1);
}
```

## Sort

### Trong thư viện <algorithm></algorithm>

- `sort(a, a + n)` : sắp xếp cả mảng theo thứ tự tăng dần
- `sort(a, a + n, greater<kiểu_dữ_liệu>)` : sắp xếp cả mảng theo thứ tự giảm dần tương tự với vector
- `sort(a + x, a + y + 1)` : sắp xếp trên [x,y] theo thứ tự tăng dần
- `sort(vi.begin(), vi.end())` : sắp xếp trên cả vector
- `sort(vi.begin() + x, vi.begin() + y + 1)` : sắp xếp trên [x, y] của vector
- Ứng dụng một số bài toán :
  ```cpp
  // Cho 1 mảng số nguyên, in ra các phần tử trong mảng theo thứ tự tuần suất xuất hiện giảm dần
  // Đối vs các pt có cùng tần suất thì pt nào nhỏ hơn thì in ra trước
  #include <bits/stdc++.h>
  using namespace std;
  using ll = long long;

  bool cmp(pair<int, int> a, pair<int, int> b){
      if(a.second != b.second) return a.second > b.second;
      else return a.first < b.first;
  }

  int main(){
      freopen("input.txt", "r", stdin);
      freopen("output.txt", "w", stdout);
      ios::sync_with_stdio(false); 
      cin.tie(nullptr); cout.tie(nullptr);

      map<int, int> mp;
      int n; cin >> n;
      int a[n];
      for(int i = 0; i < n; i++){
          cin >> a[i];
          mp[a[i]]++;
      }
      vector<pair<int, int>> vi;
      for(auto x : mp) vi.push_back({x.first, x.second});
      sort(vi.begin(), vi.end(), cmp);
      for(auto x : vi) cout << x.first << ' ' << x.second << endl;

  }

  //Sắp xếp theo trị tuyệt đối tăng dần, nếu số có cùng trị tuyệt đối thì in ra theo thứ tự xuất hiện ban đầu
  #include <bits/stdc++.h>
  using namespace std;
  using ll = long long;

  bool cmp(int a, int b){
      return abs(a) < abs(b);
  }

  int main(){
      freopen("input.txt", "r", stdin);
      freopen("output.txt", "w", stdout);
      ios::sync_with_stdio(false); 
      cin.tie(nullptr); cout.tie(nullptr);
      //sử dụng stable_sort
      int n; cin >> n;
      int a[n];
      for(int &x : a) cin >> x;
      stable_sort(a, a + n, cmp);
      for(int x : a) cout << x << ' ';

      //ko dùng stable_sort
      int n; cin >> n;
      //mảng các pair
      pair<int, int> a[n];
      //gán giá trị cho first và chỉ số cho second -> những số cùng TTD -> sx theo chỉ số đánh dấu
      for(int i = 0; i < n; i++){
          cin >> a[i].first;
          a[i].second = i;
      }
      sort(a, a + n, cmp);
      for(auto x : a) cout << x.first << ' ';
  }
  ```

### Sort cơ bản

#### Selection sort

- Cài đặt :
  ```cpp
  // Gán chỉ số min
  void selection_sort(int a[], int n){
      for(int i = 0; i < n - 1; i++){
          //Giả sử chỉ số ở phần tử i là nhỏ nhất
          int min = i;
          for(int j = i + 1; j < n; j++){
              //Duyệt thấy thằng nào nhỏ hơn min thì cập nhật chỉ số min = j
              if(a[min] > a[j]) min = j;
          }
          //Đổi chô 2 phần tử cho nhau
          swap(a[i], a[min]);
          cout << "Buoc thu " << i + 1 << ": ";
          for(int i = 0; i < n; i++) cout << a[i] << " ";
      }
      cout << endl;
      for(int i = 0; i < n; i++) cout << a[i] << ' ';
  }
  ```

#### Insertion sort

- Cài đặt :
  ```cpp
  //Chèn những phần tử nằm sai vị trí vào vị trí đúng của nó -> xét 1 phần tử và những phần tử đứng trước nó xem thử nó đứng đúng thứ tự chưa
  void insertion_sort(int a[], int n){
      for(int i = 1; i < n; i++){
          //Lấy ra phần tử ở chỉ số i và vị trí trước phần tử trước nó
          int x = a[i], pos = i - 1;
          while(pos >= 0 && a[pos] > x){
              //Dịch sang phải để chừa chỗ để chèn x vào vị trí pos
              a[pos + 1] = a[pos];
              pos--;
          }
          //out vòng while có 2 trường hợp : chạy hết chỉ số pos (pos = -1) hoặc x > a[pos] => chèn sau vị trí của a[pos]
          a[pos + 1] = x;
          cout << "Buoc thu " << i << ": ";
          for(int i = 0; i < n; i++) cout << a[i] << " ";
      }
      cout << endl;
      for(int i = 0; i < n; i++) cout << a[i] << " ";
  }
  ```

#### Counting sort

- Cài đặt :
  ```cpp
  // Cách 1
  int cnt[10000001] = {0};
  void counting_sort(int a[], int n){ 
      int min_val = INT_MAX, max_val = INT_MIN;
      for(int i = 0; i < n; i++){
          cnt[a[i]]++;
          max_val = max(max_val, a[i]);
          min_val = min(min_val, a[i]);
      }
      for(int i = min_val; i <= max_val; i++){
          if(cnt[i] != 0){
              while(cnt[i]--) cout << i << " ";
          }
      }
  }

  // Cách 2
  const int maxn = 1e6 + 5;
  int b[maxn] = {0};
  void countingSort(int a[], int n, int c[]){
      for(int i = 0; i < n; i++) b[a[i]]++;
      for(int i = 1; i <= maxn; i++) b[i] += b[i - 1];
      for(int i = n - 1; i >= 0; i--){
          b[a[i]]--;
          c[b[a[i]]] = a[i];
      }
  }
  ```
- Ứng dụng Counting sort : Frequency theo thứ tự xuất hiện của các phần tử trong mảng
  ```cpp
  int cnt[10000001] = {0};
  int used[10000001] = {0};
  void freq(int a[], int n){
      for(int i = 0; i < n; i++) cnt[a[i]]++;  
      for(int i = 0; i < n; i++){
          if(used[a[i]] == 0){
              cout << a[i] << " " << cnt[a[i]] << endl;
              used[a[i]] = 1;
          } 
      }
  }
  ```

#### Interchange và Bubble Sort

```cpp
//Thấy thằng nào nhỏ hơn số đang xét là đổi chổ luôn
void interchange_sort(int a[], int n){
    for(int i = 0; i < n - 1; i++){
        for(int j = i + 1; j < n; j++){
            if(a[i] > a[j]) swap(a[i], a[j]);
        }
        cout << "Buoc thu " << i + 1 << ": ";
        for(int i = 0; i < n; i++) cout << a[i] << " ";
    }
    cout << endl;
    for(int i = 0; i < n; i++) cout << a[i] << ' ';
}

//Đưa phần tử lớn nhất nổi về sau cùng -> xét 1 cặp a[j] và a[j + 1] ứng vs mỗi chỉ số i
void bubble_sort(int a[], int n){
    for(int i = 0; i < n - 1; i++){
        for(int j = 0; j < n - i - 1; j++){
            if(a[j] > a[j + 1]) swap(a[j], a[j + 1]);
        }
        cout << "Buoc thu " << i + 1 << ": ";
        for(int i = 0; i < n; i++) cout << a[i] << " ";
    }
    cout << endl;
    for(int i = 0; i < n; i++) cout << a[i] << " ";
}
```

### Sort nâng cao

#### Merge sort

- Dùng khi cần ổn định, sắp xếp dữ liệu lớn, hoặc khi dữ liệu không nằm hoàn toàn trong bộ nhớ
- Cài đặt :

  - Dùng vector :

  ```cpp
  //Trộn 2 dãy con đã sắp xếp vào 1 dãy : dãy 1 [l, m], dãy 2 [m + 1, r]
  void merge(int a[], int l, int m, int r){
      //copy nội dung ra 2 mảng con 1 và 2
      vector<int> x(a + l, a + m + 1); //mảng con 1
      vector<int> y(a + m + 1, a + r + 1); // mảng con 2
      //Trộn vào mảng a thành 1 dãy tăng dần bắt đầu từ chỉ số l
      int i = 0, j = 0;
      while(i < x.size() && j < y.size()){
          if(x[i] <= y[j]){
              a[l] = x[i];
              l++; i++;
          }
          else{
              a[l] = y[j];
              l++; j++;
          }
      }
      //Trộn các phần tử còn dư (nếu có) của mỗi mảng vào mảng a
      while(i < x.size()){
          a[l] = x[i];
          l++; i++;
      }
      while(j < y.size()){
          a[l] = y[j]; 
          l++; j++;
      }
      //vì chỉ số left chỉ truyền tham trị nên sau vòng lặp while thì chỉ số left ko đổi
  }
  //Cài đặt hàm mergeSort theo đệ quy => chia 2 dãy con và sắp xếp 2 dãy con đó theo thứ tự tăng dần => trộn 2 dãy đó vào 1 dãy
  void merge_sort(int a[], int l, int r){
      //Chia dãy thành các dãy con bằng đệ quy và sắp xếp theo thứ tự tăng dần
      if(l >= r) return;
      int m = (l + r) / 2;
      merge_sort(a, l, m);
      merge_sort(a, m + 1, r);
      //Trộn 2 dãy vào 1 dãy
      merge(a, l, m, r);
  }
  void print(int a[], int n){
      for(int i = 0; i < n; i++) cout << a[i] << ' ';
      cout << endl;
  }
  ```

  - Dùng mảng :

  ```cpp
  void merge(int a[], int l, int m, int r){
      int cnt = l;
      int n1 = m - l + 1, n2 = r - m;
      int x[n1], y[n2];
      for(int j = l; j <= m; j++) x[j - l] = a[j];
      for(int j = m + 1; j <= r; j++) y[j - m - 1] = a[j];
      int i = 0, j = 0;
      while(i < n1 && j < n2){
          if(x[i] <= y[j])
              a[cnt++] = x[i++];
          else
              a[cnt++] = y[j++];
      }
      while(i < n1) a[cnt++] = x[i++];
      while(j < n2) a[cnt++] = y[j++];
  }
  void merge_sort(int a[], int l, int r){
      if(l < r){
          int m = (l + r) / 2;
          merge_sort(a, l, m);
          merge_sort(a, m + 1, r);
          merge(a, l, m, r);
      }
  }
  void print(int a[], int n){
      for(int i = 0; i < n; i++) cout << a[i] << ' ';
      cout << endl;
  }
  ```
- Ứng dụng :

  - Đếm số lượng cặp nghịch thế (Count inversion)

    - Brute force (O(n^2))

    ```cpp
    int count_inversion(int a[], int n){
        int dem = 0;
        for(int i = 0; i < n; i++){
            for(int j = i + 1; j < n; j++){
                if(a[i] > a[j]) dem++;
            }
        }
        return dem;
    }
    ```

    - Merge sort (O(NlogN))

    ```cpp
    int merge(int a[], int l, int m, int r){
        //sao chép nội dung 2 dãy con
        vector<int> x(a + l, a + m + 1);
        vector<int> y(a + m + 1, a + r + 1);
        //trộn 2 dãy con thành 1 dãy từ chỉ số l
        int i = 0, j = 0;
        //Đếm số lượng cặp nghịch thế a[i] > a[j]
        int cnt = 0;
        while(i < x.size() && j < y.size()){
            if(x[i] <= y[j]){
                //ko tạo cặp nghịch thế
                a[l] = x[i];
                l++; i++;
            }
            else{
                // tạo cặp nghịch thế => số cặp nghịch thế sẽ tính từ chỉ số i đến x.size()
                cnt += x.size() - i;
                a[l] = y[j];
                l++; j++;
            }
        }
        while(i < x.size()){
            a[l] = x[i];
            l++; i++;
        }
        while(j < y.size()){
            a[l] = y[j];
            l++; j++;
        }
        return cnt;
    }
    int mergeSort(int a[], int l, int r){
        int dem = 0;
        if(l < r){
            int m = (l + r) / 2;
            //count inversion = số lượng cặp nghịch thế đoạn bên trái + số lượng cặp nghịch thế đoạn bên phải + số lượng cặp nghịch thế ở phần tử bên trái và bên phải (trộn 2 dãy con thành 1 dãy)
            dem += mergeSort(a, l, m);
            dem += mergeSort(a, m + 1, r);
            dem += merge(a, l, m, r);
        }
        return dem;
    }
    ```
  - In mergeSort theo format và liệt kê theo từng bước để biết rõ cách nó hoạt động

    ```cpp
    #define vi vector<int>
    #define vvi vector<vector<int>>
    #define p pair<int, int>

    stringstream ss;

    void input(vi &container) {
       for (int i = 0; i < container.size(); i++) {
           cin >> container[i];
       }
    }

    void saveChangeContainer(vi &container, int left, int right) {
       for (int i = 0; i < container.size(); i++) {
           if (i == left) {
               ss << "[ " << container[i] << ' ';
           } else if (i == right) {
               ss << container[i] << " ] ";
           } else {
               ss << container[i] << ' ';
           }
       }
       ss << '\n';
    }

    void merge(vi &container, int left, int right, int partition) {
       vi vectorLeft(container.begin() + left, container.begin() + partition + 1);
       vi vectorRight(container.begin() + partition + 1, container.begin() + right + 1);

       int i = 0, j = 0, indexInContainer = left;
       while (i < vectorLeft.size() && j < vectorRight.size()) {
           if (vectorLeft[i] <= vectorRight[j]) {
               container[indexInContainer] = vectorLeft[i];
               i++; indexInContainer++;
           } else {
               container[indexInContainer] = vectorRight[j];
               j++; indexInContainer++;
           }
       }

       while (i < vectorLeft.size()) {
           container[indexInContainer] = vectorLeft[i];
           i++; indexInContainer++;
       }

       while (j < vectorRight.size()) {
           container[indexInContainer] = vectorRight[j];
           j++; indexInContainer++;
       }
    }

    void mergeSort(vi &container, int left, int right) {
       if (left >= right) return;

       int partition = (left + right) / 2;

       mergeSort(container, left, partition);
       mergeSort(container, partition + 1, right);

       merge(container, left, right, partition);
       saveChangeContainer(container, left, right);
    }

    int main() {
       int n;
       cin >> n;
       vi container(n);
       input(container);
       mergeSort(container, 0, container.size() - 1);
       cout << ss.str();
    }
    ```

    - Natural Merge Sort : sort theo đường chạy

    ```cpp
    // Find runs
    vector<vector<int>> find_runs(int a[], int n){
        vector<vector<int>> runs;
        vector<int> curr;
        curr.push_back(a[0]);
        for(int i = 1; i < n; i++){
            if(a[i] >= a[i - 1]){ // sắp xếp tăng dần hoặc giảm dần sửa chỗ này
                curr.push_back(a[i]);
            }
            else{
                runs.push_back(curr);
                curr.clear();
                curr.push_back(a[i]);
            }
        }
        runs.push_back(curr);
        return runs;
    }
    // Merge
    vector<int> merge(vector<int> &x, vector<int> &y){
        vector<int> res;
        int i = 0, j = 0;
        while(i < x.size() && j < y.size()){
            if(x[i] <= y[j]){ //sắp xếp tăng dần hoặc giảm dần sửa chỗ này
                res.push_back(x[i]);
                i++;
            }
            else{
                res.push_back(y[j]);
                j++;
            }
        }
        while(i < x.size()){
            res.push_back(x[i]);
            i++;
        }
        while(j < y.size()){
            res.push_back(y[j]);
            j++;
        }
        return res;
    }
    void natural_merge_sort(int a[], int n, vector<int> &res){
        vector<vector<int>> runs = find_runs(a, n);
        while(runs.size() > 1){
            vector<vector<int>> merge_run;
            for(int i = 0; i < runs.size(); i += 2){
                if(i + 1 < runs.size()){
                    merge_run.push_back(merge(runs[i], runs[i + 1]));
                }
                else merge_run.push_back(runs[i]);
            }
            runs = merge_run;
        }
        res = runs[0];
    }
    ```

#### Heap sort

- Dùng khi muốn đảm bảo thời gian sắp xếp 𝑂(𝑛log𝑛)O(nlogn) trong mọi trường hợp và cần ít bộ nhớ phụ.
- node gốc : 0
- mỗi node cha : (i - 1) / 2
- node con bên phải : 2i + 1
- node con bên trái : 2i + 2
- Tạo Max heap : Heapify
  ```cpp
  void heapify(int a[], int n, int i){
      //xem node đang xét là lớn nhất
      int largest = i; 
      int l = 2 * i + 1;
      int r = 2 * i + 2;
      //kiểm tra node con bên trái và bên phải vẫn thỏa chỉ số trong mảng và nếu nó lớn hơn node cha thì gán chỉ số
      if(l < n && a[l] > a[largest]) largest = l;
      if(r < n && a[r] > a[largest]) largest = r;
      if(largest != i){
          // nếu node i ko phải max thì swap vs node largest
          swap(a[i], a[largest]);
          //tiếp tục gọi đệ quy đến node chỉ số largest 
          heapify(a, n, largest); 
      }
  }
  void buildHeap(int a[], int n){
      //gọi heapify với mọi node ko phải node lá => Node cha đầu tiên i = (i - 1) / 2
      for(int i = n / 2 - 1; i >= 0; i++) heapify(a, n, i);
  }
  ```
- Cài đặt
  ```cpp
  //Heap sort
  //Tạo Max heap -> Heapify
  void heapify(int a[], int n, int i){
      int largest = i; //xem node đang xét là lớn nhất
      int l = 2 * i + 1;
      int r = 2 * i + 2;
      //kiểm tra node con bên trái và bên phải vẫn thỏa chỉ số trong mảng và nếu nó lớn hơn node cha thì gán chỉ số
      if(l < n && a[l] > a[largest]) largest = l;
      if(r < n && a[r] > a[largest]) largest = r;
      if(largest != i){
          // nếu node i ko phải max thì swap vs node largest
          swap(a[i], a[largest]); 
          //tiếp tục gọi đệ quy đến node chỉ số largest
          heapify(a, n, largest); 
      }
  }
  //swap(a[0], a[n - 1])
  //Heapify lại từ cái node gốc sau khi swap phần tử lớn nhất vs phần tử cuối trong đoạn từ 0 đến n - 2
  void heap_sort(int a[], int n){
      //tạo heapify
      //gọi heapify với mọi node ko phải node lá => Node cha đầu tiên i = i / 2 - 1
      for(int i = n / 2 - 1; i >= 0; i--) heapify(a, n, i);
      // xét từ cuối đến đầu
      for(int i = n - 1; i >= 0; i--){  
          // đổi chỗ phần tử lớn nhất
          swap(a[0], a[i]); 
          // tạo max heap với số lượng phần tử là i vì mỗi lần swap sẽ ko xét đến thằng cuối cùng nữa
          heapify(a, i, 0); 
      }
  }
  ```
- Code khác
  ```cpp
  void heapify(int a[], int k, int n){
      int j = 2 * k + 1;
      while(j < n){
          if (j + 1 < n){
              if (a[j] < a[j + 1]) j = j + 1;
          }
          if(a[k] >= a[j]) return;
          swap(a[k], a[j]);
          k = j; 
          j = 2 * k + 1;
      }
  }
  void buildHeap(int a[], int n){
      int i = (n - 1) / 2;
      while(i >= 0){
          heapify(a, i, n);
          i--;
      }
  }
  void heapsort(int a[], int n){
      buildHeap(a, n);
      while(n > 0){
          n = n - 1;
          swap(a[0], a[n]);
          heapify(a, 0, n);
      }
  }
  ```

#### Quick sort

- Dùng khi cần hiệu suất cao, in-place và dữ liệu ngẫu nhiên, nhưng cần chú ý chọn pivot tốt
- Hoare Partition :
  ```cpp
  //Tìm cặp nghịch thế (a[i] > a[j]) i = l - 1, j = r + 1 -> swap cặp đó luôn
  int partitionHoare(int a[], int l, int r){
      int pivot = a[l]; // phần tử đầu
      int i = l, j = r;
      while(1){
          while(a[i] < pivot) i++;
          while(a[j] > pivot) j--;
          // kết thúc vòng while là 1 cặp nghịch thế, nếu 2 phần tử này vẫn còn thỏa điều kiện thì swap luôn
          if(i < j){ 
              swap(a[i], a[j]);
              i++; j--;
          }
          // j lúc này ko phải nằm giữa dãy nữa mà là 1 phần tử của dãy con bên trái để xét tiếp đệ quy 
          else return j; 
      }
  }
  void quick_sort_Hoare(int a[], int l, int r){
      if(l >= r) return;
      int p = partitionHoare(a, l, r); // phân hoạch 2 dãy con
      quick_sort_Hoare(a, l, p); // phần tử p phải thuộc dãy con trái
      quick_sort_Hoare(a, p + 1, r); // dãy con bên phải
  }
  ```
- Lomuto Partition :
  ```cpp
  //Nếu dãy đã xếp tăng dần thì ko tối ưu : Chọn pivot = r -> duyệt từ l đến r - 1 -> nếu phần tử nào < pivot thì swap, còn > pivot thì bỏ qua
  int partitionLomuto(int a[], int l, int r){
      int pivot = a[r]; // phần tử ngoài cùng
      int i = l - 1; // biến i 1 trong 2 biến dùng để chạy và hoán đổi vị trí của 2 phần tử
      for(int j = l; j < r; j++){
          if(a[j] <= pivot){
              i++;
              swap(a[i], a[j]);
          }
      }
      //đưa chốt về giữa
      i++;
      swap(a[i], a[r]);
      return i; // vị trí chốt sau khi phân hoạch
  }
  void quick_sort_Lomuto(int a[], int l, int r){
      if(l >= r) return;
      //phân hoạch thành 2 dãy con
      int p = partitionLomuto(a, l, r);
      //đệ quy dãy bên trái
      quick_sort_Lomuto(a, l, p - 1);
      //đệ quy dãy bên phải
      quick_sort_Lomuto(a, p + 1, r);
  } 
  ```

## Search

### Linear Search

- Cài đặt :
  ```cpp
  bool linearSearch(int a[], int n int x){
      for(int i = 0; i < n; i++){
          if(a[i] == x) return true;
      }
      return false;
  }
  ```

### Binary Search

- Cài đặt :

  ```cpp
  //áp dụng vs mảng đã sx tăng dần
  bool binarySearch(int a[], int n, int x){
      int l = 0, r = n - 1;
      while(l <= r){
          int m = (l + r)/2;
          if(a[m] == x) return true; //tìm thấy
          else if(a[m] < x) l = m + 1; //tìm bên phải mid
          else r = m - 1; // tìm bên trái mid
      }
      return false; // ko tìm thấy 
  }
  ```
- Tìm kiếm nội suy phát triển từ Binary Search

  ```cpp
  //áp dụng vs mảng đã sx tăng dần
  int interSearch(int a[], int n, int x){
      int l = 0, r = n - 1;
      while(l <= r){
          int m = l + ((r - l) * (x - a[l]) / (a[r] - a[l]));
          if(a[m] == x) return m; //tìm thấy
          else if(a[m] < x) l = m + 1; //tìm bên phải mid
          else r = m - 1; // tìm bên trái mid
      }
      return -1; // ko tìm thấy 
  }
  ```
- Ứng dụng Binary Search :

  - Cho 1 mảng đã sắp xếp tăng dần tìm xem số lượng phần tử = x xuất hiện trong mảng
  - **Dùng 2 hàm tìm first pos và last pos kết hợp công thức total = last - first + 1**
  - Chú ý trường hợp `l = -1 và r = -1`
    ```cpp
    if(l != -1) total = last - first + 1;
    else total = 0;
    ```
  - Tìm vị trí đầu tiên của x trong mảng đã sắp xếp tăng dần

  ```cpp
  int firstPos(int a[], int n, int x){
      int res = -1, l = 0, r = n - 1;
      while(l <= r){
          int m = (l + r) / 2;
          if(a[m] == x){
              res = m;
              r = m - 1; // tiếp tục tìm kiếm bên trái mid xem còn x ko
          }
          else if(a[m] < x) l = m + 1; // tìm kiếm bên phải mid
          else r = m - 1;
      }
      return res;
  }
  ```

  - Tìm vị trí cuối cùng của x trong mảng đã sắp xếp tăng dần

  ```cpp
  int lastPos(int a[], int n, int x){
      int res = -1, l = 0, r = n - 1;
      while(l <= r){
          int m = (l + r) / 2;
          if(a[m] == x){
              res = m;
              l = m + 1; //tiếp tục tìm bên phải mid
          }
          else if(a[m] > x) r = m - 1; //tìm bên trái mid
          else l = m + 1;
      }
      return res;
  }
  ```

  - Tìm vị trí đầu tiên của phần tử >= x

  ```cpp
  int lowerBound(int a[], int n, int x){
      int l = 0, r = n - 1, res = -1;
      while(l <= r){
          int m = (l + r) / 2;
          if(a[m] >= x){
              res = m;
              r = m - 1;
          }
          else l = m + 1;
      }
      return res;
  }
  ```

  - Tìm vị trí đầu tiên của pt > x

  ```cpp
  int upperBound(int a[], int n, int x){
      int l = 0, r = n - 1, res = -1;
      while(l <= r){
          int m = (l + r) / 2;
          if(a[m] > x){
              res = m;
               r = m - 1;
          }
          else l = m + 1;
      }
      return res;
  }
  ```

## Brute force

Vét cạn -> xét mọi cấu hình, trường hợp cụ thế => Solution

### Generation

- Bài toán liệt kê : Xác định cấu hình đầu tiên và cấu hình cuối cùng + Thuật toán sinh
- Mã giả :
  ```cpp
   <Khởi tạo cấu hình đầu tiên>
   while(<Chưa phải cấu hình cuối cùng>){
   		<Xử lý cấu hình hiện tại>
   		<Sinh ra cấu hình kế tiếp>
   }
  ```

#### Sinh nhị phân có độ dài n

- Bản chất sinh ra cấu hình nhị phân kế tiếp : cộng thêm 1 vào cấu hình hiện tại → có thể hiểu là dịch bit sang trái liên tục gặp bit 1 đổi thành 0 + gặp bit 0 lần đầu tiên thì dừng và viết lại các phần trước xuống lại nếu còn
- Dùng mảng → lưu dãy bit của cấu hình số đang xét và thường số phần tử của mảng đó ko quá 100 (10 < n < 15 bit → a[100])
- Liệt kê các tập con có n phần tử => liệt kê xâu nhị phân
- Chia mảng/ tập hợp thành 2 phần → xâu nhị phân ⇒ gom bit 1 thành 1 tập con và bit 0 thành 1 tập con (mỗi tập là 1 xâu nhị phân)

```cpp
//Sinh tất cả các xâu nhị phân có độ dài n
int n, a[100], check; // n độ dài bit nhị phân, mảng chứa các bit nhị phân, check có phải cấu hình cuối chưa

void khoiTao(){
    //Sinh ra xâu nhị phân đầu tiên toàn bộ là 0
    for(int i = 1; i <= n; i++){ 
        a[i] = 0;
    }
}

void sinh(){
    //bắt đầu từ bit cuối cùng n dịch qua
    int i = n;
    while(i >= 1 && a[i] == 1){ //chừng nào còn thỏa điều kiện là các bit = 1 và chưa hết xâu nhị phân
        a[i--] = 0; //Đổi các bit đó sang 0
    }
    if(i == 0){ //cấu hình cuối toàn là 1 thì chạy hết xâu
        check = 0; // dừng chương trình
    }
    else a[i] = 1; // bật bit 0 lần đầu tiên lên là 1
}

int main(){
    cin >> n;
    check = 1;
    khoiTao();//Tạo cấu hình đầu tiên
    while(check){
        for(int i = 1; i <= n; i++) cout << a[i]; //in cấu hình đầu tiên
        cout << endl; // sau mỗi cấu hình thì xuống dòng
        sinh(); // sinh ra các cấu hình tiếp theo 
    }
}
```

#### Sinh tổ hợp chập k của n phần tử

- Cấu hình đầu tiên là k số nhỏ nhất trong n số **VD n = 5, k = 3 → 123**
- Cấu hình cuối cùng là k số lớn nhất trong n số **VD n = 5, k = 3 → 345 có công thức : n - k + i**
- Sinh ra tổ hợp tiếp theo phải lớn hơn tổ hợp hiện tại nhưng phải bé nhất theo thứ tự từ điển nên khi gặp bit đầu tiên chưa đạt max **n - k + i** thì tăng bit đó lên 1 đơn vị và xét kể từ bit đó về sau mỗi bit j tương ứng sẽ +1 đơn vị lên

```cpp
//Sinh tổ hợp chập k của n pt
int n, k, a[100], check;
void khoiTao(){
    for(int i = 1; i <= k; i++) a[i] = i; //k bit bé nhất
}

void sinh(){
    int i = k;
    while(i >= 1 && a[i] == n - k + i){
        i--; // Nếu mỗi bit đạt giá trị max r thì dịch qua bit tiếp theo
    }
    if(i == 0) check = 0; //Đạt cấu hình cuối
    else{
        a[i]++; //bit đầu tiên chưa đạt max tăng 1 đv
        for(int j = i + 1; j <= k; j++){ //xét từ bit thứ i+1 đến bit thứ k
            a[j] = a[j - 1] + 1; //giá trị từ sau pt i+1 tăng 1 đv
        }
    }
}

int main(){
    cin >> n >> k;
    check = 1;
    khoiTao();
    while(check){
        for(int i = 1; i <= k; i++) cout << a[i];
        cout << endl;
        sinh();
    }
}
```

#### Sinh hoán vị

- Số có n chữ số sinh ra 1 số lớn hơn số đó và là nhỏ nhất có thể
- Điều kiện là **ít nhất trong 1 số có n chữ số** có **1 cặp số sao cho số đứng trước < số đứng sau** ⇒ hoán vị 2 chữ số
- Xét từ cuối về → tìm 1 số a[i] < a[i + 1] ⇒ xét đoạn [i+1, n] tìm đc a[j]_min > a[i] ⇒ sort đoạn đó tạo dãy tăng dần
- Sinh hoán vị có ứng dụng rất nhiều → có 2 hàm có sẵn :
  - `next_permuation()` : mảng, vector, string ⇒ true hoặc false
    ```cpp
    // sinh ra tất cả cấu hình bằng hàm có sẵn
    do{
        cout << s << endl;
    }while(next_permutation(s.begin(), s.end())
    ```
  - `prev_permutation()` : sinh ra hoán vị ngược từ cuối về đầu cú pháp giống `next_permutation()` **VD: 54321 → 12345**

```cpp
//Sinh hoán vị
int n, k, a[100], check;

void khoiTao(){
    //Khởi tạo dãy đầu tiên
    for(int i = 1; i <= n; i++) a[i] = i;
}

void sinh(){
    int i = n - 1; //sau n ko có pt nên phải xét từ n - 1
    while(i >= 1 && a[i] > a[i + 1]){ //Xét pt trc > pt sau
        i--;
    }
    if(i == 0) check = 0; //cấu hình cuối
    else{
        // đi tìm a[j] trong i + 1 -> n, > a[i] và nhỏ nhất có thể
        // trong đoạn i + 1 -> n tạo thành dãy giảm dần
        int j = n;
        while(a[i] > a[j]) j--; //xét từ pt cuối trở lên vì là dãy giảm dần nên pt đầu tiên lớn hơn a[i] là pt nhỏ nhất
        swap(a[i], a[j]); //đổi chỗ 2 pt cho nhau
        reverse(a + i + 1, a + n + 1); //Đảo ngược dãy trên đoạn từ i + 1 -> n => dãu tăng dần
    }
}

int main(){
    cin >> n;
    check = 1;
    khoiTao();
    while(check){
        for(int i = 1; i <= n; i++){
            cout << a[i];
        }
        cout << endl;
        sinh();
    }
}
```

#### Sinh phân hoạch

- Biểu diễn **n = tổng các số tự nhiên nhỏ hơn hoặc bằng n** → sinh ra các cách biểu diễn **(sinh hoặc quay lui)** , đếm các cách biểu diễn **(quy hoạch động)**
- Cấu hình đầu là n, cấu hình cuối là tổng n số 1 với nhau
- Xét từ bit cuối nếu là 1 thì dịch sang tiếp đến khi nào gặp số i có thể giảm đc 1 đơn vị → xem trước số i còn bao nhiêu số thì gán số lượng số còn lại cho biến dem → lấy tổng các số còn lại đó + (i - 1) = temp → lấy n - temp = kq thì lấy kq % (i-1) = q => biểu diễn số lượng bit kết quả q lần

```cpp
//Sinh phân hoạch 
int n, a[100], check, dem;//đếm số hạng của mỗi lần sinh ra cấu hình mới

void khoiTao(){
    dem = 1;//số lượng pt là 1
    a[1] = n;
}

void sinh(){
    int i = dem; //bắt đầu sinh từ số hạng cuối
    while(i >= 1 && a[i] == 1){ // nếu các bit là số 1 thì bỏ qua đến số có thể giảm 1 dv đc
        i--;
    }
    if(i == 0) check = 0; //cấu hình cuối full số hạng 1
    else{
        a[i]--;
        int thieu = dem - i + 1; // vì dem = số lần bỏ qua các số 1 mà trc đó giảm thêm 1 đv của a[i] nên phải +1
        dem = i; //cập nhật lại số phần tử hiện tại
        int q = thieu / a[i]; // tìm xem thiếu gấp bao nhiêu lần a[i] để biểu diễn q lần số a[i] ở vế sau
        int r = thieu % a[i]; // tương tự như trên nhưng mà là bit còn sót lại khi ko còn biểu diễn bằng a[i] đc nữa
        if(q != 0){
            for(int j = 1; j <= q; j++){ //biểu diễn q lần a[i]
                dem++; 
                a[dem] = a[i];
            }
        }
        if(r != 0){
            dem++; 
            a[dem] = r;
        }
    }
}

int main(){
    cin >> n;
    check = 1;
    khoiTao();
    while(check){
        for(int i = 1; i <= dem; i++) cout << a[i] << ' ';
        cout << endl;
        sinh();
    }
}
```

### Mã Gray

#### Chuyển từ mã nhị phân sang mã Gray

- Thuật toán
  - bit đầu tiên của mã Gray và mã nhị phân là giống nhau
  - các bit còn lại ở vị trí i của mã Gray có được bằng cách XOR 2 bit thứ i và i - 1 của xâu nhị phân

```cpp
void convert(string s){
    cout << s[0];
    for(int i = 1; i < s.size(); i++){
        //XOR bit s[i], s[i - 1]
        if(s[i] == s[i - 1]) cout << 0;
        else cout << 1;
    }
    cout << endl;
}
```

#### Chuyển từ mã Gray sang mã nhị phân

- Thuận toán
  - bit đầu tiên của mã Gray và mã nhị phân là giống nhau
  - các bit thứ i còn lại có được bằng cách:
    - nếu bit thứ i của mã Gray là 0 thì bit thứ i của mã nhị phân là copy của bit thứ i - 1 của mã nhị phân
    - Ngược lại, bit thứ i của mã nhị phân là lật ngược của bit thứ i của bit thứ i - 1 của mã nhị phân

```cpp
void convert(string s){
    string res = "";
    res += s[0];
    for(int i = 1; i < s.size(); i++){
        if(s[i] == '1'){
            if(res[i - 1] == '0') res += "1";
            else res += "0";
        }
        else res += res[i - 1];
    }
    cout << res << endl;
}
```

## Backtracking

- Quay lui → đệ quy + bài toán liệt kê mà ko biết cấu hình cuối cùng hoặc đầu tiên + ko biết thuật toán sinh ra cấu hình kế tiếp ⇒ thử mọi TH => Solution
- Tính số lần gọi đệ quy bằng chia thành các nhánh của số các giá trị mà mỗi thành phần có thể nhận đc
- Chú ý mỗi nhánh nó sẽ thực hiện đi sâu nhất có thể khi mà ko được thì nó mới chẻ sang nhánh tiếp theo rồi tiếp tục dò ⇒ không thực hiện đồng thời 2 nhánh
- Mã giả :
  ```cpp
  X = {x1, x2, x3,... xn} -> cần xây dựng X gồm các pt
  Try(int i){
  	//i gán các giá trị cho thành phần xây dựng nên X -> thử cho đến khi thỏa
  	for(j = <khả năng 1>; j <= <khả năng m>; j++){
          x[i] = j;
          if(i == n){ //xây dựng đc đên xn
              <In kq> //cấu hình cuối
          }
          else Try(i + 1)
  	}
  }
  ```
- Mã giả tổng quát
  ```cpp
  X = {x1, x2, x3,... xn} -> cần xây dựng X gồm các pt
  Try(int i){
  	//i gán các giá trị cho thành phần xây dựng nên X -> thử cho đến khi thỏa
  	for(j = <khả năng 1>; j <= <khả năng m>; j++){
          if(<có thể gán j cho X[i]>){
              x[i] = j;
              <Ghi nhận j đã đc sử dụng>
              if(i == n){ //xây dựng đc đên xn
                  <In kq> //cấu hình cuối
              }
              else Try(i + 1);
              <Backtrack>
              <bỏ ghi nhận đã sử dụng cho các nhánh khác thử tiếp giá trị j>
          }
  	}
  }
  ```

### Sinh xâu nhị phân có độ dài n

```cpp
//Sinh xâu nhị phân có n bit -> Backtracking
int n, X[100];

void Try(int i){
    //đi xây dựng bit thứ i cho xâu nhị phân
    for(int j = 0; j <= 1; j++){ //Xâu nhị phân chỉ có 2 khả năng là 0 vs 1
        X[i] = j; //bit thứ i = khả năng j
        if(i == n){ //bit cuối cùng
            for(int i = 1; i <= n; i++) cout << X[i]; //in xâu nhị phân
            cout << endl;
        } 
        else{
            Try(i + 1); // chưa phải bit cuối thì gọi đệ quy bit tiếp theo
        }
    }
}

int main(){
    cin >> n;
    Try(1);
}
```

### Sinh tổ hợp châp k của n

```cpp
//Sinh tổ hợp chập k của n pt
int n, k, X[100]; 
//Giới hạn được khả năng của i theo công thức X[i - 1] + 1 < X[i] <= n - k + i
//vì cấu hình tổ hợp chập k của n thì giá trị đứng sau > đứng trc -> X[i - 1] <= X[i]
//nhưng vì mảng X[100] là lưu toàn cục nên giá trị ban đầu là 0 phải + 1
void Try(int i){
    for(int j = X[i - 1] + 1; j <= n - k + i; j++){
        X[i] = j;
        if(i == k){
            for(int i = 1; i <= k; i++) cout << X[i];
            cout << endl;
        }
        else Try(i + 1);
    }
}

int main(){
    cin >> n >> k;
    Try(1);
}
```

### Sinh hoán vị

```cpp
//Sinh hoán vị -> khả năng nhận được sẽ là từ 1 -> n ko giới hạn
int n, X[100];
//Làm như v thì sẽ bị lắp giá trị vì i thử hết mọi giá trị có thể của j nên lặp giá trị

// Hoán vị lặp
void Try(int i){
    for(int j = 1; j <= n; j++){
        X[i] = j;
        if(i == n){
            for(int i = 1; i <= n; i++) cout << X[i];
            cout << endl;
        }
        else Try(i + 1);
    }
}

//Hoán vị ko lặp
bool used[100]; // KT giá trị i đã đc sử dụng chưa

void Try(int i){
    for(int j = 1; j <= n; j++){
        if(used[j] == false){ //Check giá trị j được gán cho các pt X[1] -> X[i-1]
            X[i] = j; // Giá trị j đã đc sử dụng
            used[j] = true; // Báo cho các lời đệ quy ở sau rằng giá trị j đã được sử dụng
            if(i == n){
                for(int i = 1; i <= n; i++) cout << X[i];
                cout << endl;
            }
            else Try(i + 1);
            //Backtrack : khi đi hết nhánh mà gán giá trị j cho X[i] -> trả lại giá trị j cho các nhánh khác sử dụng
            used[j] = false;
        }
    }
}

int main(){
    cin >> n;
    Try(1);
}
```

## Divide and Conquer

### Xâu fibo

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll f[100];
char fiboFind(ll n, ll k){
    if(n == 1) return 'A';
    if(n == 2) return 'B';
    // xâu thứ n - 2 vị trí k
    if(k <= f[n - 2]) return fiboFind(n - 2, k);
    // xâu thứ n - 1 vị trí k - f[n - 1]
    else return fiboFind(n - 1, k - f[n - 2]);
}
int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    // So sánh chỉ số k cần tìm thuộc xâu fibo thứ n - 2 hoặc n - 1 rồi gọi đệ quy tới
    f[0] = 0;
    f[1] = 1;
    for(int i = 2; i <= 92; i++) f[i] = f[i - 1] + f[i - 2];
    ll n, k; cin >> n >> k;
    cout << fiboFind(n, k) << endl;
}
```

### Số Fibonacci thứ N

- N rất lớn không dùng QHD được vì n = 10^10
- Lũy thừa nhị phân ma trận ma trận

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int mod = 1000000007;
struct mtx{
    ll a[2][2];
    friend mtx operator * (mtx x, mtx y){
        mtx res;
        for(int i = 0; i < 2; i++){
            for(int j = 0; j < 2; j++){
                res.a[i][j] = 0;
                for(int k = 0; k < 2; k++){
                    res.a[i][j] += (x.a[i][k] * y.a[k][j]);
                    res.a[i][j] %= mod;
                }
            }
        }
        return res;
    }
};
mtx binPow(mtx x, ll n){
    // Đệ quy
    if(n == 1) return x;
    mtx X = binPow(x, n / 2);
    if(n % 2 == 1) return X * X * x;
    else return X * X;

    // Ko đệ quy  
    // ma trận đơn vị
    mtx res;
    res.a[0][0] = 1;
    res.a[0][1] = 0;
    res.a[1][0] = 0;
    res.a[1][1] = 1;
    while(n){
        if(n % 2 == 1) res = res * x;
        x = x * x;
        n /= 2;
    }
    return res;
}

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    mtx x;
    x.a[0][0] = 1;
    x.a[0][1] = 1;
    x.a[1][0] = 1;
    x.a[1][1] = 0;
    ll n; cin >> n;
    mtx res = binPow(x, n);
    cout << res.a[0][1] << endl;
}
```

### Đếm dãy số

- Kỹ thuật star and bar => tính lũy thừa nhị phân với 2^(n-1)
- Không dùng sinh với quay lui được vì n = 10^12

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll binpow(ll a, ll b){
    if(b == 0) return 1;
    ll X = binpow(a, b / 2);
    if(b % 2 == 0) return X * X;
    else return a * X * X;
}

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int n; cin >> n;
    cout << binpow(2, n - 1) << endl;
}
```

## Greedy

- Tham lam: Tự build lên từ những kiến thức tự nhiên (constructive) -> Sắp xếp trước khi thực hiện
  - Ứng viên: Tập dữ liệu đề bài => Suy ra lời giải cho bài toán
  - Hàm lựa chọn: lựa chọn ứng viên tốt nhất
  - Hàm khả thi: quyết định xem ứng viên có được chọn hay không
  - Hàm mục tiêu: xác định xem giá trị của lời giải chưa hoàn chỉnh của từng bước là tốt nhất chưa
  - Hàm đánh giá: xác định xem lời giải đã được hoàn thành hay chưa

### Coin problem

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int n; cin >> n;
    // Sắp xếp trước
    int a[10] = {1000, 500, 200, 100, 50, 20, 10, 5, 2, 1};
    int res = 0;
    for(int i = 0; i < 10; i++){
        res += n / a[i];
        n %= a[i];
    }
    cout << res << endl;
}
```

### Scheduling

```cpp
// Xếp lịch diễn
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int n; cin >> n;
    pair<int, int> a[n];
    for(auto &x : a){
        cin >> x.first >> x.second;
    }
    sort(a, a + n, [](auto a, auto b){
        if(a.first != b.first) return a.second < b.second;
        else return a.first < b.first;
    });
    int cnt = 1, endTime = a[0].second;
    for(int i = 1; i < n; i++){
        if(a[i].first > endTime){
            cnt++;
            endTime = a[i].second;
        }   
    }
    cout << cnt << endl;
}
```

### Nối dây

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int t; cin >> t;
    while(t--){
        // Dùng Min heap
        int n; cin >> n;
        priority_queue<int, vector<int>, greater<int>> q;
        for(int i = 0; i < n; i++){
            int x; cin >> x;
            q.push(x);
        }
        ll res = 0;
        while(q.size() > 1){
            int x = q.top();
            q.pop();
            int y = q.top();
            q.pop();
            res += x + y;
            q.push(x + y);
        }
        cout << res << endl;

        // Dùng multiset
        int n; cin >> n;
        multiset<int> se;
        for(int i = 0; i < n; i++){
            int x; cin >> x;
            se.insert(x);
        }
        ll res = 0;
        while(se.size() > 1){
            auto i = *se.begin();
            se.erase(se.begin());
            auto j = *se.begin();
            se.erase(se.begin());
            res += i + j;
            se.insert(i + j);
        }
        cout << res << endl;
    }
}
```

## Dynamic Programming

- Phân chia bài toán lớn thành nhiều bài toán con
- Có thể kết hợp lời giải, đáp án của những bài toán con để tạo thành đáp án cho bài toán lớn
  - Base case
  - Công thức truy hồi
- Có không gian vật lý lưu trữ đáp án của các bài toán: bảng phương án (bảng 2 chiều)
- LIS
- Subset sum

# Python
