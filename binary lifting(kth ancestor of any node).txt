#include <bits/stdc++.h>

using namespace std;

//Debug Function-->works for vector,map,set(dbg(stl_name)),,arrar(dbg(array_name,range))
template < typename F, typename S >ostream& operator << ( ostream& os, const pair< F, S > & p ) {return os << "(" << p.first << ", " << p.second << ")";}
template < typename T >ostream &operator << ( ostream & os, const vector< T > &v ) {os << "{";for(auto it = v.begin(); it != v.end(); ++it) {if( it != v.begin() ) os << ", ";os << *it;}return os << "}";}
template < typename T >ostream &operator << ( ostream & os, const set< T > &v ) {os << "[";for(auto it = v.begin(); it != v.end(); ++it) {if( it != v.begin() ) os << ", ";os << *it;}return os << "]";}
template < typename T >ostream &operator << ( ostream & os, const multiset< T > &v ) {os << "[";for(auto it = v.begin(); it != v.end(); ++it) {if( it != v.begin() ) os << ", ";os << *it;}return os << "]";}
template < typename F, typename S >ostream &operator << ( ostream & os, const map< F, S > &v ) {os << "[";for(auto it = v.begin(); it != v.end(); ++it) {if( it != v.begin() ) os << ", ";os << it -> first << " = " << it -> second ;}return os << "]";}
#define dbg(args...) do {cerr << #args << " : "; arif(args); } while(0)
clock_t tStart = clock();
#define timeStamp dbg("Execution Time: ", (double)(clock() - tStart)/CLOCKS_PER_SEC)
void arif () {cerr << endl;}
template <typename T>void arif( T a[], int n ) {for(int i = 0; i < n; ++i) cerr << a[i] << ' ';cerr << endl;}
template <typename T, typename ... hello>void arif( T arg, const hello &... rest) {cerr << arg << ' ';arif(rest...);}

#define db long long
#define ll long long
#define int ll
#define vi vector<int>
#define vs vector<string>
#define vpi vector<pair<int, int>>
#define vpd vector<pair<double, double>>
#define vps vector<pair<string, string>>
#define mp(x, y) make_pair(x, y)
#define pb(x) push_back(x)
#define pp(x) pop_back(x)
#define fr(cont) for (auto &i : (cont))
#define all(cont) cont.begin(), cont.end()
#define rall(cont) cont.rbegin(), cont.rend()

int const N = 2e5+9;
vi tree[N];
int up[N][20];//These information allow us to jump from any node to any ancestor above it in  O(log(N)) time

void binary_lifting(int src,int par)
{
    up[src][0] = par;//2^0 = 1 node uprer ancestor src er parent nijei
    for(int i = 1;i <  20;i++)//2^20 == 1e6..
    {
        if(up[src][i-1] == -1)
            up[src][i] = -1;
        else
            up[src][i] = up[up[src][i-1]][i-1];//u node theke 2^x node uporer node = up[src][2^x-1][2^x-1]..2^x = 2^x-1 + 2^x-1
    }
    fr(tree[src])
    {
        if(i != par)
            binary_lifting(i,src);
    }
}

//per query TC-->O(log(n))
int query(int v,int k){
    if(v == -1 or k == 0)
        return v;
    for(int i = 19;i >=0;i--)
    {
        if(k >= (1<<i))
            return query(up[v][i],k-(1<<i));
    }
}

void solve()
{
    int n,q;
    cin>> n >> q;
    for(int i= 2;i <= n;i++)
    {
        int x;
        cin>>x;
        tree[x].pb(i);
        tree[i].pb(x);
    }
    binary_lifting(1,-1);//root 1 er upore ar kono lvl nai.tai er jonno parent -1
    while(q--)
    {
        int v,k;
        cin >>v >> k;
        cout << query(v,k) << endl;
    }
}   

int32_t main()
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
    //Learnt from this video https://www.youtube.com/watch?v=FAfSArGC8KY&list=WL&index=15
    https://cses.fi/problemset/task/1687/
*/