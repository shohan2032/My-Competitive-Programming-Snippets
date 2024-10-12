#include <bits/stdc++.h>

using namespace std;

#define db long long
#define ll long long
#define vi vector<int>
#define vs vector<string>
#define vl vector<ll>
#define pii pair<int, int>
#define vpi vector<pair<int, int>>
#define vpl vector<pair<ll, ll>>
#define vpd vector<pair<double, double>>
#define vps vector<pair<string, string>>
#define mp(x, y) make_pair(x, y)
#define f first
#define s second
#define pb(x) push_back(x)
#define pp(x) pop_back(x)
#define loop1 for (i = 0; i < n; i++)
#define loop2 for (j = 0; j < n; j++)
#define fr(cont) for (auto &i : (cont))
#define all(cont) cont.begin(), cont.end()
#define rall(cont) cont.rbegin(), cont.rend()
#define inv a, b, i, j, k, l, r, t, q, cnt1 = 0, cnt2 = 0, x, y, z, n, cnt = 0, ma = INT_MIN, mi = INT_MAX, sum = 0
#define sv s1, s, s2, s3, s4
#define sv s1, s, s2, s3, s4

// constants...
const double pi = acos(-1);
const ll mod = 1000000007;
const int MXS = 2e5 + 5;
const ll MXI = 1e9 + 5;
const ll MXL = 1e18 + 5;
const ll INF = 1e9 + 5;
const ll INFLL = 1e18 + 5;
const ll eps = 1e-9;

// functions...
ll gcd(ll a, ll b)
{
    while (b)
    {
        a %= b;
        swap(a, b);
    }
    return a;
}
ll lcm(ll a, ll b) { return (a / gcd(a, b) * b); }
ll ncr(ll a, ll b)
{
    ll x = max(a - b, b), ans = 1;
    for (ll K = a, L = 1; K >= x + 1; K--, L++)
    {
        ans = ans * K;
        ans /= L;
    }
    return ans;
}
ll bigmod(ll a, ll b, ll mod)
{
    if (b == 0)
    {
        return 1;
    }
    ll tm = bigmod(a, b / 2, mod);
    tm = (tm * tm) % mod;
    if (b % 2 == 1)
        tm = (tm * a) % mod;
    return tm;
}
ll egcd(ll a, ll b, ll &x, ll &y)
{
    if (a == 0)
    {
        x = 0;
        y = 1;
        return b;
    }
    ll x1, y1;
    ll d = egcd(b % a, a, x1, y1);
    x = y1 - (b / a) * x1;
    y = x1;
    return d;
}
ll modpow(ll a, ll p, ll mod)
{
    ll ans = 1;
    while (p)
    {
        if (p % 2)
            ans = (ans * a) % mod;
        a = (a * a) % mod;
        p /= 2;
    }
    return ans;
}
ll inverse_mod(ll n, ll mod) { return modpow(n, mod - 2, mod); }
// ll fact[2000005];
// ll ncr_mod(ll n,ll r) {return (((fact[n]*inverse_mod(fact[r]))%mod)*inverse_mod(fact[n-r]))%mod;}
db pytha(db x1, db x2, db y1, db y2) { return sqrt((pow((x2 - x1), 2)) + (pow((y2 - y1), 2))); } /// for two ending points pythagoras law
double make_radian(double x) { return (x * pi) / 180; }
double make_degree(double x) { return (x * 180) / pi; }
ll binmul(ll a, ll b, ll p)
{
    ll res = 0ll;
    while (b)
    {
        if (b & 1)
            res = (res + a) % p, b--;
        else
            a = (a + a) % p, b /= 2;
    }
    return res;
} //(a*b)%p
ll binpow(ll a, ll b, ll p)
{
    ll res = 1ll;
    while (b)
    {
        if (b & 1)
            res = binmul(res, a, p), b--;
        else
            a = binmul(a, a, p), b /= 2;
    }
    return res;
} //(a^b)%p
ll inverse(ll a, ll p) { return binpow(a, p - 2, p); } //(a^-1)%p == (a^p-2)%p

/*string decToBinary(int n)
{
    string s;
    int i = 0;
    while (n > 0)
    {
        s = to_string(n % 2) + s;
        n = n / 2;
        i++;
    }
    return s;
}*/

/*ll binaryToDecimal(string n)
{
    string num = n;
    ll dec_value = 0;
    int base = 1;
    int len = num.length();
    for (int i = len - 1; i >= 0; i--)
    {
        if (num[i] == '1')dec_value += base;
        base = base * 2;
    }
    return dec_value;
}*/

/*ll factorial(int n)
{
    long long fact = 1;  int md = 998244353;
    for (int i = 1; i <= n; i++)
    {
        fact = (fact * i) % md;
    }
    return fact;
}*/

/*BS
    cin >> y;
    bool found = false;
    l = 0;
    r = n-1;
    while(l <= r)
    {
      m = (l+r)/2;
      if(vc[m] == y)
      {
        found = true;
        break;
      }
      else if(y > vc[m])
      {
        l = m+1;
      }
      else
        r = m-1;
    }
    if(found)
      cout << "YES\n";
    else
      cout << "NO\n";
*/

/*LB__UB
    l = -1,r = n;
        while(l + 1 != r)
        {
            m = l+r/2;
            if(vc[m] >= x)///if(vc[m] > x) for upper bound
                r = m;
            else
                l = m;",
        }
        cout << r;
*/
ll r,c;
ll arr[11][11];
map<ll,ll>mp;
ll ans = 0;
void recursive_path(int i,int j)
{
    if(i > r or j > c)
        return;
    if(mp[arr[i][j]] > 1)
        return;
    if(i == r and j == c)
        ans++;
    mp[arr[i][j+1]]++;
    recursive_path(i,j+1);
    mp[arr[i][j+1]]--;

    mp[arr[i+1][j]]++;
    recursive_path(i+1,j);
    mp[arr[i+1][j]]--;
}
void solve()
{
    cin >> r >> c;
    for(int i =1;i <= r;i++)
    {
        for(int j = 1;j <= c;j++)
        {  
            cin >> arr[i][j];
        }
    }
    mp[arr[1][1]]++;
    recursive_path(1,1);
    cout<<ans << endl;
}   

int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    // auto st = clock();
    int t = 1;
    // cin >> t;
    while (t--)
        solve();

    // cerr << 1.0*(clock()-st)/CLOCKS_PER_SEC << endl;
    return 0;
}
/*Problem_link
    https://atcoder.jp/contests/abc293/tasks/abc293_c
*/