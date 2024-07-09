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
    getline(cin, s);
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
### 3.WERTYU
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/63ccb5e7-fd4d-4885-a6a6-7520e6a51441)
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/48560cb3-3398-445c-a441-e610c7c3158b)

你交怎么这么喜欢考字符串的题目（）

这题没啥好说的，只需要搞清楚映射的定义域和值域就结束了。
```cpp
#include <cstdio>
#include <iostream>
#include <string>
#include <map>

using namespace std;

map<string, string> Pmap;

int main()
{
    Pmap["W"] = "Q";
    Pmap["S"] = "A";
    Pmap["X"] = "Z";
    Pmap["E"] = "W";
    Pmap["D"] = "S";
    Pmap["C"] = "X";
    Pmap["R"] = "E";
    Pmap["F"] = "D";
    Pmap["V"] = "C";
    Pmap["T"] = "R";
    Pmap["G"] = "F";
    Pmap["B"] = "V";
    Pmap["Y"] = "T";
    Pmap["H"] = "G";
    Pmap["N"] = "B";
    Pmap["U"] = "Y";
    Pmap["J"] = "H";
    Pmap["M"] = "N";
    Pmap["I"] = "U";
    Pmap["K"] = "J";
    Pmap[","] = "M";
    Pmap["O"] = "I";
    Pmap["L"] = "K";
    Pmap["."] = ",";
    Pmap["P"] = "O";
    Pmap[";"] = "L";
    Pmap["/"] = ".";
    Pmap["["] = "P";
    Pmap["'"] = ";";

    string s;
    getline(cin, s);
    for (int i = 0; i < s.length(); ++i)
    {
        string ch(1, s[i]); // 将字符转换为单字符字符串
        if (Pmap.find(ch) != Pmap.end())
        {
            cout << Pmap[ch];
        }
        else
        {
            cout << s[i];
        }
    }

    return 0;
}
```

### 4.Simple Sorting
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/d25ffbde-e69f-4999-a552-cbccc448a461)

排序去重algorithm里甚至都有内置函数。不过我们还是手写一个归并排序+手动去重吧。

```cpp
#include <cstdio>
#include <iostream>
using namespace std;
const int N = 1e3 + 5;
int n, a[N], temp[N];
int ans[N];

void mergesort(int l, int r)
{
    if(l == r)
        return;
    int m = (l + r) >> 1;
    mergesort(l, m);
    mergesort(m + 1, r);
    int ls = l, rs = m + 1, now = l;
    while(now <= r)
    {
        if(ls <= m && rs <= r)
        {
            if(a[ls] > a[rs])
                temp[now++] = a[rs++];
            else
                temp[now++] = a[ls++];
        }
        else if(ls <= m)
            temp[now++] = a[ls++];
        else
            temp[now++] = a[rs++];
    }
    for(int i = l; i <= r; ++i)
        a[i] = temp[i];
    return;
}

int main()
{
    cin>>n;
    for(int i = 1; i <= n; ++i)
        cin>>a[i];
    mergesort(1, n);
    cout<<a[1];
    int now = 2;
    while(now <= n)
    {
        while(a[now] == a[now - 1] && now <= n)
            ++now;
        if(now > n)
            break;
        cout<<" "<<a[now++];
    }
    return 0;
}
```
### 5.Old Bill
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/cc5c0902-f7ba-4b51-bd66-ebe6ab6b2504)
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/a9c1510e-c897-4eca-8f52-3cacce11a297)
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/816e1d8c-3e58-4bad-8cbe-b1ec988790a6)

讲道理这题本身是很简单的，直接枚举最高位的1~9以及低位的0~9即可。恶心的点依旧是不指定个数的输入组。纯模拟题，直接挂代码了。
```cpp
#include <cstdio>
#include <iostream>
using namespace std;
int n;
int x, y, z;

int main()
{
    while(cin>>n)
    {
        bool flag = 1;
        cin>>x>>y>>z;
        for(int i = 9; i >= 1; --i)
        {
            if(!flag)
                break;
            for(int j = 9; j >= 0; --j)
            {
                int now = i * 10000 + x * 1000 + y * 100 + z * 10 + j;
                if(now % n == 0)
                {
                    cout<<i<<" "<<j<<" "<< now / n << endl;
                    flag = 0;
                    break;
                }
            }
        }
        if(flag)
            cout<<0<<endl;
    }
    return 0;
}
```

### 6.Fibonacci
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/c15f14c2-e860-4a17-a29d-62e854beb8f5)

签到题，甚至n都只有30。

但是我们怎么能满足于30呢，毕竟直接递归的复杂度是O(2^N)，即使考虑记忆化搜索也是O(N)。我们可以用矩阵快速幂优化到O(logN)。

```cpp
#include <cstdio>
#include <iostream>
using namespace std;
int n;
struct matrix
{
    int a, b, c, d;

    matrix()
    {
        a = 1;
        b = 1;
        c = 1;
        d = 0;
    }
};
matrix operator*(const matrix &x, const matrix &y)
{
    matrix res;
    res.a = x.a * y.a + x.b * y.c;
    res.b = x.a * y.b + x.b * y.d;
    res.c = x.c * y.a + x.d * y.c;
    res.d = x.c * y.b + x.d * y.d;
    return res;
}
int main()
{
    cin>>n;
    if(n == 1 || n == 2)
    {
        cout<<1<<endl;
        return 0;
    }
    int now = n - 2;
    matrix res;
    matrix s;
    res.a = 1;
    res.b = 0;
    res.c = 0;
    res.d = 1;
    while(now)
    {
        if(now & 1)
            res = res * s;
        s = s * s;
        now >>= 1;
    }
    cout<<res.a + res.b;
    return 0;
}
```
### 7.数字反转
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/5a6da7b6-a0a3-4a64-a47b-7ea7c1fcdf33)

这题就略了，我也没想到什么要注意的点，就是个数字翻转而已，甚至高精度都用不上，无限制输入前面也涉及到了。

## 套题3（SJTU）
### 1.涂颜色
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/33449b94-1bbd-4059-bc3c-e8a2db192ea6)

看似很难的题，但是仔细分析就会发现，其实只需要考虑每一行，上下行之间是独立的。

而对于每一行，一旦第一个方块确定了，后面依次就能确定每一个方块，因此只有两种方案。

所以答案就是2的n次方。最后利用快速幂结合同余即可完成。本题代码略。

### 2.平衡字符串
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/0501ba72-e58a-4ffd-8e12-40818d8e1182)

只会最暴力的解法，摆了

### 3.最小面积子矩阵
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/e2286541-b910-49df-b8ce-63855bafcfa5)

通过枚举行，从而把问题转化为一维问题。如果所有矩阵元素非负的话就可以用双指针扫一遍解决了。

```cpp
#include<cstdio>
#include<iostream>
using namespace std;
const int N=105;
int num[N][N];
int S[N][N],now[N];
int main()
{
    int n,m,k;
    cin>>n>>m>>k;
    int ans=n*m;
    for(int i=1;i<=n;++i)
        for(int j=1;j<=m;++j)
        {
            cin>>num[i][j];
            S[i][j]=S[i-1][j]+num[i][j];
        }
    for(int i=1;i<=n;++i)
        for(int j=i;j<=n;++j)
        {
            for(int r=1;r<=m;++r)
                now[r]=S[j][r]-S[i-1][r];
            int ls=1,rs=1;
            int sum=now[1],res=m+1;
            while(ls<=rs&&rs<=m)
            {
                while(sum<k&&rs<m)  
                    sum+=now[++rs];
                while(sum>=k&&ls<=rs)
                {
                    res=min(res,rs-ls+1);
                    sum-=now[ls++];
                }
                if(sum<k&&rs==m)
                    break;
            }
            if(res<=m)
                ans=min(ans,(j-i+1)*res);
        }
    cout<<ans;
    return 0;
}
```
### 4.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/976f16ec-3385-4534-8537-bb04e8a596d4)

如果数据范围不大的话转化为背包问题就可以，用f(n)来表示是否可以用数组中的元素凑出n这个数。

当然，这个题有一点恶心的地方是，需要手动检查输入是否合法。由于C++无法像python那样先读进来再检查类型，因此我们只能先全部以string的形式读入，然后再想办法处理。
```cpp
#include<cstdio>
#include<iostream>
using namespace std;
const int N=1e5+5;
string temp;
int nums[N];
bool f[N];
bool check(string s)
{
    for(int i=0;i<s.length();++i)
    {
        if('0'<=s[i]&&s[i]<='9')
            continue;
        else
            return false;
    }
    return true;
}
int conv(string s)
{
    int res=0;
    for(int i=0;i<s.length();++i)
        res=res*10+(s[i]-'0');
    return res;
}
int main()
{
    int n=0;
    while(cin>>temp)
    {
        if(check(temp))
           nums[++n]=conv(temp);
        else
        {
            cout<<"ERROR";
            return 0;
        }
    }
    f[0]=true;
    int sum=0;
    for(int i=1;i<=n;++i)
        sum=sum+nums[i];
    for(int i=1;i<=n;++i)
    {
        for(int j=sum;j>=nums[i];--j)
            f[j]=(f[j] | f[j-nums[i]]);
        // for(int j=1;j<=8;++j)
        //     cout<<f[j]<<" ";
        // cout<<endl;
    }
    for(int i=(sum+1)/2;i<=sum;++i)
        if(f[i])
        {
            cout<<i<<" "<<sum-i;
            break;
        }
    return 0;
}
```

### 5.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/bc42b89d-7b09-461d-adca-052f8f4f95d4)

序列翻转，略

### 6.棋盘游戏
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/584fcbe9-dd2b-4036-82c0-ea69cf4a110b)

（由于数据大小固定了6*6，感觉暴力dfs似乎也可以搞定）

乍一看可能觉得这题可以记忆化搜索，然而实际上是不行的，因为不满足后效性原则。

正确的思路是把一个点劈成4份，再根据不同的状态转移规则连边建图，最后用最短路算法来完成。

```cpp
#include<cstdio>
#include<iostream>
#include<queue>
#include<cstring>
using namespace std;
const int N=1e4;
int input[10][10];
struct Edge
{
    int v, w;
    Edge(int x=0, int y=0)
    {
        v=x;
        w=y;
    }
}E[N];
int lst, fst[N], nxt[N];
void adde(int x,int y,int z)
{
    E[++lst]=Edge(y,z);
    nxt[lst]=fst[x];
    fst[x]=lst;
}
int dx[4]={1,0,-1,0};
int dy[4]={0,1,0,-1};

struct node
{
    int n;
    int dis;
    node(int nn=0,int ddis=0)
    {
        n=nn;
        dis=ddis;
    }
};

bool operator<(const node &x, const node &y)
{
    return x.dis>y.dis;
}

priority_queue<node> heap;

int dis[N];
bool inqueue[N];
void dijkstra(int ss)
{
    dis[ss]=0;
    heap.push(node(ss,0));
    while(!heap.empty())
    {
        node ttmp=heap.top();
        heap.pop();
        int nn=ttmp.n;
        int ddis=ttmp.dis;
        if(inqueue[nn])
            continue;
        dis[nn]=ddis;
        inqueue[nn]=1;
        for(int i=fst[nn];i;i=nxt[i])
        {
            int v=E[i].v;
            int w=E[i].w;
            // cout<<v<<" "<<w<<endl;
            if(!inqueue[v])
                if(dis[v]>dis[nn]+w)  
                {
                    dis[v]=dis[nn]+w;
                    heap.push(node(v,dis[v]));
                }
        }
    }
}
int main()
{
    memset(dis,0x3f,sizeof(dis));
    for(int i=1;i<=6;++i)
        for(int j=1;j<=6;++j)
            cin>>input[i][j];
    for(int i=1;i<=6;++i)
        for(int j=1;j<=6;++j)
        {
            //当前点：i,j，对应id：i*6-6+j
            int now=i*6+j-6;
            for(int s=1;s<=4;++s)
            {
                //图上编号：now*4-4+s
                int tmp=now*4-4+s;
                for(int k=0;k<4;++k)
                {
                    int nx=i+dx[k],ny=j+dy[k];
                    if(nx>0&&nx<=6&&ny>0&&ny<=6)
                    {
                        //对方id
                        int to=nx*6-6+ny;
                        int cost=s*input[nx][ny];
                        //计算目标状态
                        int ns=cost%4+1;
                        //对方点在图里的编号
                        int pmt=to*4-4+ns;
                        adde(tmp,pmt,cost);
                    }
                }
            }
        }
    
    //图建好后根据出发点跑dijkstra即可。
    int sx,sy,tx,ty;
    cin>>sx>>sy>>tx>>ty;
    sx++;
    sy++;
    tx++;
    ty++;
    int ss=(sx*6-6+sy)*4-3;
    int tt=tx*6-6+ty;
    dijkstra(ss);
    // for(int i=1;i<=144;++i)
    //     cout<<i<<" "<<dis[i]<<endl;
    int ans=dis[tt*4-4+1];
    for(int i=2;i<=4;++i)
        ans=min(ans,dis[tt*4-4+i]);
    cout<<ans;
    return 0;
}
```

### 7.取中值
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/2b548076-9038-4b18-b206-fd438b987406)

模拟+排序，略

### 8.Prime Number
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/6582e5bc-1980-4236-aac8-dbb63fb68b17)

正好复习一下质数的筛法。（虽然我估计直接暴力找/打表就可以）

```cpp
#include<cstdio>
#include<iostream>
using namespace std;
const int N=5e6+5;
bool noprime[N];
int cnt,primes[100005];
int main()
{
    int k;
    cin>>k;
    int n=N;
    for(int i=2;i<=n;++i)
    {
        if(cnt>=k)
            break;
        if(!noprime[i])
        {
            primes[++cnt]=i;
            // cout<<i<<endl;
            for(int j=i+i;j<=n;j+=i)
                noprime[j]=1;
        }
    }
    // for(int i=1;i<=cnt;++i)
    //     cout<<primes[i]<<endl;
    // cout<<k<<endl;
    cout<<primes[k];
    return 0;
}
```

### 9.图的连通分支数

![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/b3ebc9c1-9aa2-4819-b2ce-9fe35bb5db89)

这题没啥好说的，建图dfs打标记就行，主要考察建图，注意有向无向，完事。

### 10.最短路径
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/57635029-e87a-475e-9ba4-5d1d8496ea38)
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/d6520914-57e6-4e9f-84b6-ad5f764c9f00)

最短路模板题，参考之前的dijkstra算法即可。

难点在于边权一定会爆，此时可以选择高精度硬做。

但是分析知道任意一条路径，和别的路径比大小是只需要其中最大的2的幂次的。（这是2的幂次决定的，因为下2^0加到2^(n-1)也小于2^n）

由于路径长度不重复，所以合并的时候取max即可。

可以通过这个方式找到最短路径的具体点，然后再计算真正的最短路就好。

来不及敲代码了，先把思路都过一遍。

### 10.Coindence
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/bd9d0a6c-2bb3-4d9d-b7e8-deeca84597cf)

DP问题，f(i)(j)表示两个串分别考虑到i,j的最长公共序列。转移方程就很简单了，O(n^2)拿下。

### 11.整除问题
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/57953395-1fb2-4a67-b78c-6971ebaca22a)
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/8bcc26eb-e6c3-4d01-99f6-b3f93779d460)

```cpp
//your code here.
```

### 12.后缀子串排序
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/aef2bca5-e5d7-4b07-a196-76a89c3e23f7)

水题，直接硬排序就行，可以sort

### 13.最低票价
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/92c8eb43-31e6-423c-9014-40c63cee8db0)

```cpp
//your code here.
```

### 14.正方形数组的数目

```cpp
//your code here.
```

### 15.最长连续递增序列

![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/458ed09c-5889-44c5-8ac2-5833b2b0d746)

```cpp
//your code here.
```

### 16.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/7e8ec98b-6820-42be-b762-250ee216b17e)

```cpp
//your code here.
```

### 17.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/8d3bc5e9-1feb-4483-aa0b-90c90ffae92c)

```cpp
//your code here.
```

### 18.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/4a7603f9-6a7d-4ea6-8532-f67e2933934a)

略

### 19.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/9801d26b-6c3d-4aa5-b16d-f7fbcdbe241c)

和前面一个题是一样的，略

### 20.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/5851fbcb-3bfa-433a-8bd0-5ebfdaabcf41)

最简单的数学题，统计2和5的因子数就行

### 21.
![image](https://github.com/SaMMyCHoo/SummerCamp-Exercises/assets/116826455/5521238e-fc6c-4b73-bf89-a35ed1e7ee40)

用语法树递归做就行，注意优先级，没有括号所以先找加减法就行



