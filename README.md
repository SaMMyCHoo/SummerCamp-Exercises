## 套题1（PJU）

### 1. 有趣的跳跃
![image](https://github.com/SaMMyCHoo/-/assets/116826455/fbbd50fe-da1d-4e6b-9c8e-b4d2676b7e42)
![image](https://github.com/SaMMyCHoo/-/assets/116826455/cae58102-dfa9-45bc-a59f-834e6a9e49eb)

考点：模拟，排序。时间复杂度要求很低，鉴定为签到题。
思路：算出绝对差分后直接排序就行。
```cpp
#include<cstdio>
#include<iostream>
#include<algorithm>
using namespace std;
const int N=3005;
int a[N], b[N];
int main()
{
    int n, temp;
    cin>>n;
    for(int i=1;i<=n;++i)
        cin>>a[i];
    for(int i=1;i<n;++i)
    {
        temp = a[i+1]-a[i];
        if(temp<0)
            temp = -temp;
        b[i] = temp;
    }
    sort(b+1, b+n);
    for(int i=1;i<n;++i)
    {
        if(b[i]!=i)
        {
            cout<<"Not jolly";
            return 0;
        }
    }
    cout<<"Jolly";
    return 0;
}
```

### 2.玛雅历
![image](https://github.com/SaMMyCHoo/-/assets/116826455/15f9973c-9a61-4689-aa70-b0c60ef02e19)
![image](https://github.com/SaMMyCHoo/-/assets/116826455/df477227-af27-42bd-a19b-2188aa5877f0)
![image](https://github.com/SaMMyCHoo/-/assets/116826455/2f81535b-55ae-4db1-bc04-94dfd7ea85ee)

纯模拟题，题目阅读量大，考察点为阅读理解。
思路：第一个日历是正常的月份，第二个日历是天干地支类。直接算出总天数再转换即可。
```cpp
#include<cstdio>
#include<iostream>
#include<string>
using namespace std;

string Haab[19]={
    "pop", "no", "zip", "zotz", "tzec", 
    "xul", "yoxkin", "mol", "chen", "yax", 
    "zac", "ceh", "mac", "kankin", "muan",
    "pax", "koyab", "cumhu", "uayet"
    };

string Holly[20]={
    "imix", "ik", "akbal", "kan", "chicchan", "cimi", "manik",
    "lamat", "muluk", "ok", "chuen", "eb", "ben", "ix", "mem",
    "cib", "caban", "eznab", "canac", "ahau"
    };


void convert(string haab,int y)
{
    int len = haab.size();
    cout<<len<<endl;
    int temp=0, now = 0, dat, mon, yea;
    string tmp = "";
    while (now<len)
    {
        if(haab[now]=='.')
        {
            temp = 0;
            for(int i=0;i<now;++i)
                temp = temp * 10 + haab[i] - '0';
            dat = temp;
        }
        if(now==len-1)
        {
            int j = now;
            while(haab[j]!='.')
                tmp += haab[j--];
            string ttmp="";
            for(int i=tmp.length()-1;i>=0;--i)
                ttmp+=tmp[i];
            tmp=ttmp;
            // cout<<tmp<<endl;
            for(int i=0;i<19;++i)
            {
                if(Haab[i]==tmp)
                {
                    mon=i;
                    break;
                }
            }
            break;
        }
        now++;
    }
    yea = y;
    // cout<<dat<<' '<<mon<<' '<<yea<<endl;
    int tot = yea * 365 + mon * 20 + dat + 1;
    cout<<tot<<endl;
    int rd, rm, ry;
    ry = tot/260;
    rd = tot%13;
    rm = tot%20-1;
    if(rd==0)
        rd=13;

    cout<<rd<<" "<<Holly[rm]<<" "<<ry<<endl;
}

int main()
{
    int n, y;
    string temp;
    cin>>n;
    cout<<n<<endl;
    for(int i=1;i<=n;++i)
    {
        cin>>temp>>y;
        convert(temp,y);
    }
    return 0;
}
```

### 3.走迷宫

![image](https://github.com/SaMMyCHoo/-/assets/116826455/e79bd8b4-f470-4c03-ac02-85f67f541bcd)


广度优先搜索模板题，签到即可。仍然要注意字符的读入。
```cpp
#include<cstdio>
#include<iostream>
#include<string>
#include<queue>
using namespace std;
const int N = 50;
int map[N][N], dis[N][N];
int r,c;
struct node
{
    int x, y;
};

queue<node> q;

int dx[4] = {1, 0, -1, 0};
int dy[4] = {0, 1, 0, -1};

void bfs(node s)
{
    dis[s.x][s.y] = 1;
    q.push(s);
    while(!q.empty())
    {
        node tmp = q.front();
        q.pop();
        for(int i = 0; i < 4; ++i)
        {
            int nx = tmp.x + dx[i];
            int ny = tmp.y + dy[i];
            if(nx < 1 || nx > r || ny < 1 || ny > c || !map[nx][ny] || dis[nx][ny])
                continue;
            dis[nx][ny] = dis[tmp.x][tmp.y] + 1;
            node nxt;
            nxt.x = nx, nxt.y = ny;
            q.push(nxt);
        }
    }
}
int main()
{
    cin>>r>>c;
    string temp;
    for(int i = 1; i <= r; ++i)
    {
        cin>>temp;
        for(int j = 0; j < c; ++j)
        {
            char now = temp[j];
            if(now == '.')
                map[i][j + 1] = 1;
            else
                map[i][j + 1] = 0;
        }
    }
    node s;
    s.x = 1, s.y = 1;
    bfs(s);
    cout<<dis[r][c];
    return 0;
}
```
### 4.最大上升子序列和
![image](https://github.com/SaMMyCHoo/-/assets/116826455/0bd897b3-2a34-49ca-8865-6cedcc64e596)
![image](https://github.com/SaMMyCHoo/-/assets/116826455/10c6e5d9-be96-41cc-b840-d46addfeed7f)

标准的dp问题，N^2的解法并不难想。

配合下标的离散化即可优化到NlogN。

涉及到的知识点：树状数组，离散化，线性dp
```cpp
#include<cstdio>
#include<iostream>
#include<algorithm>
using namespace std;
const int N = 1e3+5;
int n, a[N];
int num[N], tmp[N];
int find(int x)
{
    int res = 0;
    int l = 1, m, r = n;
    while(l <= r)
    {
        m = (l + r) >> 1;
        res = m;
        if(tmp[m] == x)
            break;
        if(tmp[m] > x)
            r = m - 1;
        else   
            l = m + 1;
    }
    return res;
}

int tr[N]; 
int lowbit(int x)
{
    return ((x ^ (x - 1)) + 1) >> 1; 
}

void update(int a, int x)// 把下标a的最大值上调为x（单向）
{
    int now = a;
    while(now <= n)
    {
        tr[now] = max(tr[now], x);
        now += lowbit(now);
    }
}

int check(int a)
{
    int res = 0, now = a;
    while(now)
    {
        res = max(res, tr[now]);
        now -= lowbit(now);
    }
    return res;
}

int f[N];
int main()
{
    cin>>n;
    for(int i = 1; i <= n; ++i)
    {
        cin >> a[i];
        tmp[i] = a[i];
    }
    sort(tmp + 1, tmp + n + 1);
    for(int i = 1; i <= n; ++i)
        num[i] = find(a[i]);
    int ans = 0;
    for(int i = 1; i <= n; ++i)
    {
        int now = check(num[i]);
        f[i] = now + a[i];
        update(num[i], f[i]);
        ans = max(ans, f[i]);
    }
    cout<<ans;
    return 0;
}
```

### 5.Yogurt Factory
![image](https://github.com/SaMMyCHoo/-/assets/116826455/2509178e-1b1e-4a0e-beee-e63cbb15717d)
![image](https://github.com/SaMMyCHoo/-/assets/116826455/e91d7951-9ac7-40e9-b676-13bf9ceed3dc)

读懂题意后就知道是简单的贪心问题。由于酸奶和存储都没有上限，同时也不要求在线回答问题，因此我们只需要一次考虑每一周的供应。

对于第i周的供应需求，酸奶有两种来源：就当周生产的成本$C_i$，或者是之前某一周的成本$C_j + (i - j) * S$。

因此，只需要快速找出这些选项中更低的那个即可。我们整理一下表达式：$\min_{1}^{i} C_j + (i - j) * S$，等价于$\min_{1}^{i} C_j - j * S$，从而只需要维护一个静态的数组$C_j - j * S$前缀最小值即可（注意需要保留下标），因此时间复杂度是$O(n)$。

注意longlong。
```cpp
#include<cstdio>
#include<iostream>
using namespace std;
#define LL long long
const int N = 1e4+5;
int n, s, c[N], y[N];
LL ans, now;
int main()
{
    cin>>n>>s;
    for(int i = 1; i <= n; ++i)
        cin >> c[i] >> y[i];
    ans = y[1] * c[1];
    now = 1;
    for(int i = 2; i <= n; ++i)
    {
        int temp = c[now] - now * s;
        if ((c[i] - i * s) < temp)
        {
            temp = c[i] - i * s;
            now = i;
        }
        ans += y[i] * (temp + i * s);
    }
    cout<<ans;
    return 0;
}
```

### 6.Wireless Network
![image](https://github.com/SaMMyCHoo/-/assets/116826455/20e36bce-8bc9-4b24-bc2c-3861afd8783d)
![image](https://github.com/SaMMyCHoo/-/assets/116826455/8144ada7-2003-4011-ad86-7b2b380c2f86)
![image](https://github.com/SaMMyCHoo/-/assets/116826455/f9655ef4-1f63-4ffc-b40d-17282f461923)

并查集问题，直接用并查集应该是可以直接完成。难点是在于快速定位到修复点附近的已修复电脑。当然，由于只有1000台，直接遍历是可行的。

是否有更好的做法？一个简单的优化是提前预处理好拓扑关系，这样在真的连接的时候就不需要再遍历了。有效但是仍然是$O(N^2)$的复杂度。

暂时没想到更快速的方法，这里就用vector写一个好了。

```cpp
#include<cstdio>
#include<iostream>
#include<vector>
using namespace std;
#define LL long long
const int N = 1e3 + 5;
int X[N], Y[N];

vector<int> neighbors[N];

int to[N]; //并查集

int find(int x)
{
    if(x != to[x])
        to[x] = find(to[x]);
    return to[x];
}

bool check(int x, int y)
{
    x = find(x);
    y = find(y);
    return x == y;
}

void merge(int x, int y)
{
    x = find(x);
    y = find(y);
    to[x] = y;
}

int main()
{
    int n, d;
    cin >> n >> d;
    for(int i = 1; i <= n; ++i)
    {
        to[i] = -1;//初始化并查集
        cin >> X[i] >> Y[i];
    }

    for(int i = 1; i <= n; ++i)
        for(int j = i + 1; j <= n; ++j)
        {
            LL dis = (X[i] - X[j]) * 1ll * (X[i] - X[j]) + (Y[i] - Y[j]) * 1ll *(Y[i] - Y[j]);
            if(dis <= d * 1ll * d)
            {
                neighbors[i].push_back(j);
                neighbors[j].push_back(i);
            }
        }

    char temp;
    while(cin >> temp)
    {
        if(temp == 'O')
        {
            int p;
            cin >> p;
            to[p] = p;
            for(int i = 0; i < neighbors[p].size(); ++i)
                if(to[neighbors[p][i]] != -1)
                    merge(p, neighbors[p][i]);
        }
        else
        {
            int p, q;
            cin >> p >> q;
            if(to[p] == -1 || to[q] == -1)
                cout<<"FA1L"<<endl;
            if(check(p, q))
                cout<<"SUCCESS"<<endl;
            else
                cout<<"FA1L"<<endl;

        }
    }
    return 0;
}
```
## 套题2（SJTU）
### 1.String Match
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/1a26e507-0d46-454c-9be2-440e564a828e)

标准的字符串匹配问题，观察数据范围后得知需要$O(N)$完成。有两种写法，分别是KMP和哈希。KMP难度过高，所以我们选择哈希吧（）

哈希的问题在于，有一定随机性，脸黑的话容易爆。所以要选择合适的参数，以及最好双哈希保险。

```cpp
#include<cstdio>
#include<iostream>
#include<string>
using namespace std;
#define LL long long
int Hash(LL b, LL m, string s1, string s2)
{
    int ans = 0;
    LL res = 0;
    //计算s2的哈希值
    for(int i = 0; i < s2.length(); ++i)
    {
        res = (res * b) % m;
        res = (res + (int)(s2[i])) % m;
    }

    // cout<<res<<endl;

    LL tmp = 0, now = 1;
    for(int i = 0; i < s2.length(); ++i)
    {   
        if(i)
           now = (now * b) % m;
        tmp = (tmp * b) % m;
        tmp = (tmp + (int)(s1[i])) % m;
    }
    // cout<<tmp<<endl;
    for(int i = 0; i < s1.length() - s2.length(); ++i)
    {
        if(tmp == res)
            ++ans;
        LL temp = (now * s1[i]) % m;
        tmp = (tmp - temp + m) % m;
        tmp = (tmp * b) % m;
        tmp = (tmp + (int)(s1[i + s2.length()])) % m;
        // cout<<tmp<<endl;
    }
    if(tmp == res)
        ++ans;
    return ans;
}   

int main()
{
    string S1, S2;
    cin>>S1>>S2;
    int ans1 = Hash(19, 19260817, S1, S2);
    int ans2 = Hash(23, 19260817, S1, S2);
    cout<<min(ans1, ans2);
    return 0;
}
```

### 2.二次方程计算器
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/14f63241-e1ed-4781-8cc5-b0dfa75788cd)

模拟题，难点在于读入字符串后整理方程的形式。

此处假设不会出现二次计算的系数，也就是不会出现2*3x这样的项。（不然鬼才做咧）
```cpp
#include<cstdio>
#include<iostream>
#include<string>
#include<iomanip>
#include<cmath>
using namespace std;
bool isdigit(char x)
{
    return (('0' <= x) && ('9' >= x));
}
int main()
{
    string s;
    cin>>s;
    int al = 0, bl = 0, cl = 0, ar = 0, br = 0, cr = 0, a, b, c;
    int eq;
    for(int i = 0; i < s.length(); ++i)
        if(s[i] == '=')
        {
            eq = i;
            break;
        }
    //我们先找到等号，然后依次在两边确定各个项的系数。
    int now = 0, temp = 0, sign = 1;
    while(now < eq)
    {   
        if(isdigit(s[now]))
        {
            temp = temp * 10 + (s[now] - '0');
            ++now;
        }
        else 
        {
            if(s[now] == '-')
            {
                cl += temp * sign;
                sign = -1;
                temp = 0;
                ++now;
            }
            else if(s[now] == 'x')
            {
                if(!temp)
                    temp = 1;
                if(s[now + 1] == '^')
                {
                    al += temp * sign;
                    sign = 1;
                    temp = 0;
                    now += 3;
                }
                else
                {
                    bl += temp * sign;
                    sign = 1;
                    temp = 0;
                    ++now;
                }
            }   
            else
            {
                cl += temp * sign;
                sign = 1;
                temp = 0;
                ++now;
            }
        }
    }
    cl += temp * sign;
    now = eq + 1, temp = 0, sign = 1;
    while(now < s.length())
    {   
        if(isdigit(s[now]))
        {
            temp = temp * 10 + (s[now] - '0');
            ++now;
        }
        else 
        {
            if(s[now] == '-')
            {
                cr += temp * sign;
                sign = -1;
                temp = 0;
                ++now;
            }
            else if(s[now] == 'x')
            {
                if(!temp)
                    temp = 1;
                if(s[now + 1] == '^')
                {
                    ar += temp * sign;
                    sign = 1;
                    temp = 0;
                    now += 3;
                }
                else
                {
                    br += temp * sign;
                    sign = 1;
                    temp = 0;
                    ++now;
                }
            }   
            else
            {
                cr += temp * sign;
                sign = 1;
                temp = 0;
                ++now;
            }
        }
    }
    cr += temp * sign;
    // cout<<al<<" "<<bl<<" "<<cl<<" "<<endl;
    // cout<<ar<<" "<<br<<" "<<cr<<" "<<endl;
    a = al - ar;
    b = bl - br;
    c = cl - cr;
    if(!a)
    {
        if(!b)
        {
            if(!c)
                cout<<"All Solution!";
            else
                cout<<"No Solution!";
        }
        else
            cout<<fixed<<setprecision(2)<<((double)c / b);
    }
    else
    {
        int delta = b * b - 4 * a * c;
        if(delta < 0)
            cout<<"No Solution!";
        else
        {
            cout<<fixed<<setprecision(2)<<((double)(-b + sqrt(delta)) / (2.0 * a));
            cout<<" ";
            cout<<fixed<<setprecision(2)<<((double)(-b - sqrt(delta)) / (2.0 * a));
        }

    }
    return 0;
}
```
（注：其实也可以直接把这个字符串转化为表达式然后用二分法求解，但是个人感觉难度并不亚于整理系数，因为这是C++不是Python）
