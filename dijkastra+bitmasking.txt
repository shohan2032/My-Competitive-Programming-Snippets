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
#define vl vector<int>
#define pi pair<int, int>
#define vpi vector<pair<int, int>>
#define vpl vector<pair<int, int>>
#define vpd vector<pair<double, double>>
#define vps vector<pair<string, string>>
#define mp(x, y) make_pair(x, y)
#define f first
#define s second
#define pb(x) push_back(x)
#define pp(x) pop_back(x)
#define fr(cont) for (auto &i : (cont))
#define all(cont) cont.begin(), cont.end()
#define rall(cont) cont.rbegin(), cont.rend()

const ll mod = 998244353;

const int N = 2e5 + 9;
const int INF = 1e18;
vector<pair<int,int>>ad_list1[N];
int type[N];
int dijkstra(int v,int k)
{
    set<pair<int,pair<int,int>>> st;//dis,node,mask
    vector<vi> dis(v+5,vector<int> ((1ll<<k),INF));
    vector<vi> vis(v+5,vector<int> ((1ll<<k),0));

    //starting from node 1
    st.insert(mp(0,mp(1,type[1])));
    dis[1][type[1]] = 0;

    while(!st.empty())
    {
        pair<int,pair<int,int>> cur = *st.begin();
        int d = cur.first;
        int u = cur.second.first;
        int mask = cur.second.second;
        st.erase(cur);
        if(vis[u][mask])
            continue;
        vis[u][mask] = 1;
        fr(ad_list1[u])
        {
            int child = i.first;
            int wt = i.second;
            int newMask = (type[child]|mask);
            if(d+wt < dis[child][newMask])
            {
                dis[child][newMask] = d+wt;
                st.insert(mp(d+wt,mp(child,newMask)));
            }
        }
    }
    int ans = INF;
    for(int i = 0;i < (1ll << k);i++)
    {
        for(int j = 0;j < (1ll << k);j++)
        {
            if((i|j) == (1ll << k)-1)
                ans = min(ans,max(dis[v][i],dis[v][j]));
        }
    }
    return ans;
}

void solve()
{
    int v,e,k;
    cin >> v >> e >> k;
    for(int  i = 1;i <= v;i++)
    {
        int x;
        cin>>x;
        int mask = 0;
        for(int j = 1;j <= x;j++)
        {
            int type;
            cin>>type;
            mask |= (1ll << (type-1));
        }
        // cout << i << " " << mask << endl;
        type[i] = mask;//node i te je je type er fish ache oi gular set bit 1
    }
    for(int i = 1;i <= e;i++)
    {
        int u,v,w;
        cin>> u >> v >> w;
        ad_list1[u].pb(mp(v,w));
        ad_list1[v].pb(mp(u,w));
    }
    cout << dijkstra(v,k) << endl;
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
    https://www.hackerrank.com/challenges/synchronous-shopping/problem?isFullScreen=false (dijkastra+bitmasking)
*/
