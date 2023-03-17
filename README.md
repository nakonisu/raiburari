# 関数系
``` Cpp
// 累乗の計算をするpower
// power(a,b) でaのb乗

long long power(long long a, long long b) {
    if (b < 0) {
        cerr << "負の数乗。今回は逆数出したる。\n";
        return power(a, abs(b));
    } else if (b == 0) {
        return 1;
    } else if (b == 1) {
        return a;
    } else {
        return power(a, b / 2) * power(a, b / 2) * power(a, b % 2);
    }
}

// 最大公約数はgcd、標準装備！
// 最小公倍数の計算！
// lcm(a, b)でaとbの最小公倍数
long long lcm(long long a, long long b) {
    long long d = gcd(a, b);
    return a / d * b;
}


```
# 文字列操作系関数
``` cpp
// 文字列から数字へのキャスト
// stringからlonglongへのキャスト
// powerも使う！！
// strToLong(string s) でsをlonglongに変えたものを出力
long long strToLong(string s) {
    long long keta = (long long)(s).size();
    long long ans = 0;
    for (int i = keta - 1; i >= 0; i--) {
        ans += (int)(s.at(i) - '0') * power(10, keta - i - 1);
    }
    return ans;
}


// longlongをstringに変換（数字を文字列に変換）

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
string s, t;

// 文字列の反転
reverse(s.begin(), s.end());  // "abc"->"cba"

// 文字列の結合
s = "ab";
t = "cd";
s += t;  // s="abcd"

```
 
# 出力系
```cpp
// 小数点*以下*の桁数指定
// cout << fixed << setprecision(桁数) << ;
cout << fixed << setprecision(2) << 3.141;  // "3.14"
cout << fixed << setprecision(2) << 3.0;    // "3.00"

// 全体の桁数指定
// 小数部は出力されない
cout << setprecision(2) << 12.3;  // "12"
// 小数部のゼロ埋めがされない
cout << setprecision(2) << 3.0;  // "3" (3.0ではない)

// 0埋め
// cout << setw(桁数) << setfill(埋める文字) << ;
cout << setw(4) << setfill('0') << 12 << endl;       // ”0012”
cout << setfill('0') << left << std::setw(4) << 12;  // "1200"
cout << setfill(' ');  // ゼロ埋め・解除（デフォルトに戻す）

// YesNoの出力
void YesNo(bool b) {
    if (b)
        cout << "Yes" << endl;
    else
        cout << "No" << endl;
}
```

# ソートなどの操作
```cpp
vector<ll> a;
// sort ソート 別引数によるソート(2番目の要素とか)
sort(a.begin(), a.end(),
        [](const vector<ll> &alpha, const vector<ll> &beta) {
            return alpha[0] < beta[0];
        });


// 順列すべての全探索
// nextpermutation使う
long long n;  // 順列の個数(n個の並び替え)

// ここでnums を{0, 1, 2, 3...} にする
vector<int> nums(n);
for (int i = 0; i < n; i++) {
    nums[i] = i;
}

do {
    print(nums)  //{0,1,2,3,4} , {0,1,2,4,3} ...みたいになる
} while (next_permutation(nums.begin(), nums.end()));

```

# 構造体
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