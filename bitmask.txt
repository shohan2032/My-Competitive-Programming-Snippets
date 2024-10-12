//ALL POSSIBLE SUBSET
#include<bits/stdc++.h>
using namespace std;

typedef long long ll;
#define pb push_back

int main() 
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    ///complexity = O(2^n*n)all possible sub sequence with bitmask
    int n;
    vector<int>v;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        int x;
        cin>>x;
        v.push_back(x);
    }
    
    for(int mask=1;mask<(1<<n);mask++)
    {
        for(int i=0;i<n;i++)
        {
            if(mask & (1<<i))
                cout<<v[i]<<' ';
        }
        cout<<endl; 
        //__builtin_popcount(mask) --> subset er size return kore.
        // cout << "size " << __builtin_popcount(mask) << endl;    
    }
    /* input >
        3
        1 2 3 
    */
    return 0;
}
//Problem->https://codeforces.com/problemset/problem/535/B