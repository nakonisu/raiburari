# 定数
``` cpp
const long long INFLL = 1LL << 60;
const int INFINT = INT_MAX / 2;
const double PIE = acos(-1);
```

# 関数系
## 累乗の計算
- 累乗の計算をするpower
- modで割ったあまりを出力
- mod を1e18とかにすれば普通にも使える
``` cpp
// power(a,b,mod) でaのb乗のmod
long long power(long long a, long long b, long long mod = 1e18) {
    if (b < 0) {
        cerr << "負の数乗。今回は逆数出したる。\n";
        return power(a, abs(b), mod);
    } else if (b == 0) {
        return 1;
    } else if (b == 1) {
        return a % mod;
    } else {
        long long temp = power(a, b / 2, mod);
        temp = (temp * temp) % mod;
        if (b % 2 == 1) temp *= a;
        temp = temp % mod;
        return temp;
    }
}
```
## 最小公倍数
``` cpp
// lcm(a, b)でaとbの最小公倍数
long long lcm(long long a, long long b) {
    long long d = gcd(a, b);
    return a / d * b;
}
```
# 文字列操作系関数
## 文字列から数字へのキャスト
- 文字列から数字へのキャスト
- stringからlonglongへのキャスト
- **powerも使う！！**
``` cpp
// strToLong(string s) でsをlonglongに変えたものを出力
long long strToLong(string s) {
    long long keta = (long long)(s).size();
    long long ans = 0;
    for (int i = keta - 1; i >= 0; i--) {
        ans += (int)(s.at(i) - '0') * power(10, keta - i - 1, 1e18);
    }
    return ans;
}
```
## 数字から文字列へのキャスト
- longlongをstringに変換（数字を文字列に変換）

``` cpp
// longToStr(n)でnをstringに変換したものを出力
string longToStr(long long n) {
    long long k = abs(n);
    string revStr = "";
    while (k > 0) {
        revStr.push_back((char)(k % 10) + '0');
        k /= 10;
    }
    if (n < 0) revStr += "-";
    reverse(revStr.begin(), revStr.end());
    return revStr;
}
```
# 文字列操作方法
```cpp
// 文字列の反転
reverse(s.begin(), s.end());  // "abc"->"cba"

// 文字列の結合
s = "ab";
t = "cd";
s += t;  // s="abcd"
```
 
# 出力系
## 桁数指定
### 小数点*以下*の桁数指定
` cout << fixed << setprecision(桁数) << ;`
```cpp
cout << fixed << setprecision(2) << 3.141;  // "3.14"
cout << fixed << setprecision(2) << 3.0;    // "3.00"
```
### 全体の桁数指定
- 小数部は出力されない
```cpp
cout << setprecision(2) << 12.3;  // "12"
```
- 小数部のゼロ埋めがされない
``` cpp
cout << setprecision(2) << 3.0;  // "3" (3.0ではない)
```
### 0埋め
- cout << setw(桁数) << setfill(埋める文字) << ;
``` cpp
cout << setw(4) << setfill('0') << 12 << endl;       // ”0012”
cout << setfill('0') << left << std::setw(4) << 12;  // "1200"
cout << setfill(' ');  // ゼロ埋め・解除（デフォルトに戻す）

TODO 確認！！！
```
## YesNoの出力
```cpp
// YesNoの出力
void YesNo(bool b) {
    if (b)
        cout << "Yes" << endl;
    else
        cout << "No" << endl;
}
```

# ソートなどの操作方法
## 別引数のソート
```cpp
vector<ll> a;
// sort ソート 別引数によるソート(2番目の要素とか)
sort(a.begin(), a.end(),
        [](const vector<ll> &alpha, const vector<ll> &beta) {
            return alpha[1] < beta[1];
        });
```
## 順列全探索
```cpp

// 順列すべての全探索
// nextpermutation使う
// nums = {0,0,1,1}などとすることで組み合わせの全探索も可能

vector<int> nums(n);
for (int i = 0; i < n; i++) {
    nums[i] = i;
}

do {
    print(nums)  //{0,1,2,3,4} , {0,1,2,4,3} ...みたいになる
} while (next_permutation(nums.begin(), nums.end()));

```
## upper_bound,lower_bound
```cpp
// ソートが必要！！
// lower_bound
// key以上の要素の内の一番左側のイテレータを返す
// a.begin()引けばkeyより小さい要素の個数がわかる
// a.end()から引けばkey以上の個数がわかる

vector<int> a = {1, 3, 3, 4, 5};
auto iter1 = lower_bound(a.begin(), a.end(), 0);  // iter1 - a.begin() = 0
auto iter2 = lower_bound(a.begin(), a.end(), 2);  // iter2 - a.begin() = 1
auto iter3 = lower_bound(a.begin(), a.end(), 3);  // iter3 - a.begin() = 1
auto iter4 = lower_bound(a.begin(), a.end(), 6);  // iter4 - a.begin() = 5

// ソートが必要！！
// upper_bound
// keyより大きい要素の内の一番左側のイテレータを返す
// a.begin()引けばkey以下の個数がわかる
// a.end()から引けばkeyより大きい個数がわかる

auto iter5 = upper_bound(a.begin(), a.end(), 0);  // iter5 - a.begin() = 0
auto iter6 = upper_bound(a.begin(), a.end(), 2);  // iter6 - a.begin() = 1
auto iter7 = upper_bound(a.begin(), a.end(), 3);  // iter7 - a.begin() = 3
auto iter8 = upper_bound(a.begin(), a.end(), 6);  // iter8 - a.begin() = 5

// upper-lowerでその要素の個数がわかる
// iter5-iter1 = 0
// iter7-iter3 = 2
```
# 構造体
## UnionFind
```cpp
//UnionFind!!!
struct unionfind {
    // 自分の親をvectorで保存
    vector<ll> p;
    // 木の高さを保持
    vector<ll> rank;
    // 同じ木の個数
    vector<ll> size;

    // 初期化
    unionfind(ll n) {
        p.resize(n);
        rank.resize(n);
        size.resize(n);
        for (int i = 0; i < n; i++) {
            p.at(i) = i;
            rank.at(i) = 0;
            size.at(i) = 1;
        }
    }
    // xの親を返す関数
    int find(ll x) {
        if (p.at(x) == x) {
            return x;
        } else {
            return p.at(x) = find(p.at(x));
        }
    }
    // x,yを結合
    void unite(ll x, ll y) {
        ll findx = find(x);
        ll findy = find(y);
        ll rankx = rank.at(findx);
        ll ranky = rank.at(findy);
        if (findx == findy) return;
        if (rankx > ranky) {
            p.at(findy) = findx;
            size.at(findx) += size.at(findy);
        } else {
            p.at(findx) = findy;
            if (rankx == ranky) rank.at(findx)++;
            size.at(findy) += size.at(findx);
        }
    }
    // 同じ木か判定
    bool same(ll x, ll y) { return (find(x) == find(y)) ? true : false; }
};
```

# 数学
## 素数列挙（エラトステネス）
``` cpp
// エラトステネスの篩、素数列挙(鉄則本p160)
// Nまでの素数を列挙してくれる
vector<long long> makePrimeVec(int N) {
    vector<bool> deleted(N + 1, false);
    vector<ll> P = {};
    for (int i = 2; i <= N; i++) {
        if (!deleted[i]) {
            for (int j = i * i; j <= N; j += i) {
                deleted[j] = true;
            }
            P.emplace_back(i);
        }
    }

    return P;
}
```

## 素数判定1 試し割り
``` cpp

// 試し割り素数判定、nが素数かどうかを判定する
bool isPrime(long long n) {
    for (long long i = 2; i * i <= n; i++) {
        if (n % i == 0) return false;
    }
    return true;
}
```
## 素数判定2ミラーラビン（高速！！！）
```cpp
// Miller-Rabin 素数判定
// https://algo-method.com/tasks/513/editorial から拝借
// powerが必要！
bool isPrime(long long N) {
    if (N <= 1) return false;
    if (N == 2) return true;
    if (N % 2 == 0) return false;
    vector<long long> A = {2, 325, 9375, 28178, 450775, 9780504, 1795265022};
    long long s = 0, d = N - 1;
    while (d % 2 == 0) {
        ++s;
        d >>= 1;
    }
    for (auto a : A) {
        if (a % N == 0) return true;
        long long t, x = power(a, d, N);
        if (x != 1) {
            for (t = 0; t < s; ++t) {
                if (x == N - 1) break;
                x = __int128_t(x) * x % N;
            }
            if (t == s) return false;
        }
    }
    return true;
}
```

## 階乗、二項係数、順列の場合の数
- けんちょんさんのを拝借
- https://drken1215.hatenablog.com/entry/2018/06/08/210000
- maxを決めた前処理が必要
- 階乗のMODも計算できる（副産物?）
- **MOD確認** (MODは素数！)

### 前処理の計算
``` cpp
// nCrを計算するための前処理、maxまでの階乗modをメモ化

// fac->階乗,finv->階乗の逆元,inv->単に逆数？
vector<long long> fac, finv, inv;
const long long MOD = 1000000007;
// テーブルを作る前処理
void COMinit(ll max) {
    fac.resize(max);
    finv.resize(max);
    inv.resize(max);
    fac[0] = fac[1] = 1;
    finv[0] = finv[1] = 1;
    inv[1] = 1;
    for (int i = 2; i < max; i++) {
        fac[i] = fac[i - 1] * i % MOD;
        inv[i] = MOD - inv[MOD % i] * (MOD / i) % MOD;
        finv[i] = finv[i - 1] * inv[i] % MOD;
    }
}
```
### 二項係数の計算
- **COMinitも必要**
``` cpp
// nCr(n,r)で二項係数を計算
long long nCr(long long n, long long r) {
    if (n < r) {
        cerr << "n < r please" << endl;
        return 0;
    }
    if (n < 0 || r < 0) {
        cerr << "n>=0 & r>=0 please" << endl;
        return 0;
    }
    return fac[n] * (finv[r] * finv[n - r] % MOD) % MOD;
}
```
### 順列の計算！！
-  **COMinitも必要**
``` cpp
// nPr(n,r)で順列を計算
long long nPr(long long n, long long r) {
    if (n < r) {
        cerr << "n < r please" << endl;
        return 0;
    }
    if (n < 0 || r < 0) {
        cerr << "n>=0 & r>=0 please" << endl;
        return 0;
    }
    return fac[n] * finv[n - r] % MOD;
}
```
### 重複組合せの計算！！
   **COMinitも必要**
``` cpp
// nHr(n,r)で重複組合せを計算
long long nHr(long long n, long long r) {
    if (n < 0 || r < 0) {
        cerr << "n>=0 & r>=0 please" << endl;
        return 0;
    }
    if (n == 0 & r == 0) return 1;
    return nCr(n + r - 1, r);
}
```
