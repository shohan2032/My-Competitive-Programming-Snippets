#include <bits/stdc++.h>

using namespace std;

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
vector<pair<int,int>>ad_list1[N],ad_list2[N];


vector<int> dijkstra(int source,vector<int>&cnt,vpi ad_list[])
{
    priority_queue<pi,vpi,greater<pi>> q;
    vector<bool>vis(N,0);
    vi dis(N,INF);
    dis[source] = 0;
    q.push(mp(0,source));
    cnt[source] = 1;
    
    while(!q.empty())
    {
        pi top = q.top();
        int u = top.second;
        q.pop();
        if(vis[u])
            continue;
        vis[u] = 1;
        for(auto &child: ad_list[u])
        {
            int v = child.first;
            int wt = child.second;
            if(dis[u] + wt < dis[v])
            {
                dis[v] = dis[u]+wt;
                q.push(mp(dis[v],v));
                cnt[v] = cnt[u];
            }
            else if(dis[u] + wt == dis[v])
                cnt[v] = (cnt[u]+cnt[v])%mod;
        }
    }
    return dis;
}

void solve()
{
    int v,e,s,t;
    cin >> v >> e >> s >> t;
    int uu[e+5],vv[e+5],w[e+5];
    for(int i = 1;i <= e;i++)
    {
        cin >> uu[i] >> vv[i] >>w[i];
        ad_list1[uu[i]].pb(mp(vv[i],w[i]));
        ad_list2[vv[i]].pb(mp(uu[i],w[i]));
    }

    vector<int>cnt1(v+5,0),cnt2(v+5,0);
    auto dis1 = dijkstra(s,cnt1,ad_list1);
    auto dis2 = dijkstra(t,cnt2,ad_list2);
    
    // for(int i = 1;i <= v;i++)
    //     cout << i << " " << cnt1[i] << " " << cnt2[i] << endl;

    for(int i = 1;i <= e;i++){
        int u = uu[i];
        int v = vv[i];
        int wt = w[i];
        int total = dis1[u]+dis2[v]+wt;
        if(dis1[t] == total and (cnt1[u]*cnt2[v])%mod == cnt1[t])
            cout << "YES" << endl;
        else{
            int komabo = dis1[t]-(dis1[u]+dis2[v]+1);
            if(komabo>=1)   
                cout << "CAN " << wt-komabo << endl;
            else    
                cout << "NO" << endl;
        }
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
    https://codeforces.com/problemset/problem/20/C (getting shortest path between 1 and n in a weighted graph)

    https://codeforces.com/contest/450/problem/D
*/