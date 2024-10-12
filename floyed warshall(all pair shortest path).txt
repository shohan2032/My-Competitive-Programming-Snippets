#include <bits/stdc++.h>

using namespace std;
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

int const N = 510;
int const INF = 1e18;
int dis[N][N];

//FLoyd Warshall ALgo
//It can handle negative weighted graph but can not handle negative weighted cyclic graph
//TC->O(n^3)
void solve()
{
    int n,e;
    cin>> n >> e;

    for(int i =1;i <= n;i++)
    {
        for(int j=1;j <= n;j++)
        {
            if(i == j)
                dis[i][j] = 0;
            else
                dis[i][j] = INF;
        }
    }
    while(e--)
    {
        int u,v,wt;
        cin >> u >> v >> wt;
        dis[u][v] = wt;
        //dist[v][u] = wt; undirected graph hole eitau hobe
    }
    for(int k = 1;k <= n;k++)//k is the level or nodes.every time we allow a path i to j to reach through k nodes.
    {
        for(int i = 1;i <= n;i++)
        {
            for(int j = 1;j <= n;j++)
            {
                if(dis[i][k] != INF and dis[k][j] != INF)
                    dis[i][j] = min(dis[i][j],dis[i][k]+dis[k][j]);
            }
        }
    }
    for(int i = 1;i <= n;i++)
    {
        for(int j = 1;j <= n;j++)
        {
            if(dis[i][j] == INF)
                cout << "I ";
            else
                cout << dis[i][j] << " ";
        }
        cout << endl;
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
        https://codeforces.com/problemset/problem/1213/G
        https://codeforces.com/contest/25/problem/C
       https://www.spoj.com/problems/ARBITRAG/
       https://codeforces.com/gym/101845/problem/C
       https://codeforces.com/contest/33/problem/B
*/