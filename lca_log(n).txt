#include <bits/stdc++.h>

using namespace std;

#define db long long
// #define ll long long
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
#define fr(cont) for (auto &i : (cont))
#define all(cont) cont.begin(), cont.end()
#define rall(cont) cont.rbegin(), cont.rend()

const int N = 1e5 + 9;
vi ad_list[N];
int level[N];
int up[N][20];

void dfs(int u,int par,int lvl)//TC-->BigO(v+e)
{
    level[u] = lvl;
    fr(ad_list[u]){
        if(i != par)//this line can be deleted if we delete the 47th line also
            dfs(i,u,lvl+1);
    }
}

//This function TC-->O(Nlog(N))
void binary_lifting(int src,int par)
{
    up[src][0] = par;
    for(int i = 1;i < 20;i++)
    {
        if(up[src][i-1] == -1)
            up[src][i] = -1;
        else
            up[src][i] =up[up[src][i-1]][i-1];
    }
    fr(ad_list[src])
    {
        if(i != par)
            binary_lifting(i,src);
    }
}

int lift_node(int u,int jump)//jump hobe 2 er power akare
{
    if(u == -1 or jump == 0)
        return u;
    for(int i = 19;i >=0;i--)
    {
        if(jump >= (1 << i))
            return lift_node(up[u][i],jump-(1<<i));
    }
    return u;
}

//TC-->O(log(n))
int lca(int u,int v)
{
    if(level[u] < level[v])//root theke jar lvl beshi tare u te rakhtesi
        swap(u,v);
    u = lift_node(u,level[u]-level[v]);//Now u and v are at the same level
    if(u == v)//dont be confused.we made the level same not u and v
        return u;
    for(int i = 19;i >= 0;i--)
    {
        if(up[u][i] != up[v][i])//jump from u to n1 and from v to n2 untill n1 != n2.when n1 == n2 then we reach from u and v to 1 step lower node of our desired lca node.
        {
            u = up[u][i];
            v = up[v][i];
        }
    }
    return lift_node(u,1);//Now u is one step lower node of lca.so we just need 1 jump to reach in LCA.
}

void solve()
{
    int v,q;
    scanf("%d",&v);
    for(int i = 1;i <= v;i++)
    {
        ad_list[i].clear();
        level[i] = 0;
    }
    memset(up,-1,sizeof up);
    for(int i= 2;i <= v;i++)
    {
        int x,y;
        scanf("%d %d",&x,&y); 
        ad_list[x].pb(y);
        ad_list[y].pb(x);
    }
    cin>> q;
    dfs(1,-1,0);//root 1 er kono par nai so or par -1,ar or lvl 0
    binary_lifting(1,-1);
    while(q--)
    {
        int u,v;
        scanf("%d %d",&u,&v);
        int lc = lca(u,v);
        cout << lc << endl;
    }
}   

int main()
{
    // ios_base::sync_with_stdio(false);
    // cin.tie(NULL);
    // cout.tie(NULL);
    // auto st = clock();
    int t = 1;
    // scanf("%d",&t);
    while (t--)
        solve();

    // cerr << 1.0*(clock()-st)/CLOCKS_PER_SEC << endl;
    return 0;
}
/*Problem_link
    https://cses.fi/problemset/task/1688/
 https://codeforces.com/blog/entry/55274
 LCA with binary lifting+MST -->
    https://codeforces.com/problemset/problem/609/E
    https://www.hackerrank.com/contests/srbd-code-contest-2024-round-1/challenges/terror-in-terminus
*/