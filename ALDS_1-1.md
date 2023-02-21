# ALDS1-1

## 1.挿入ソート

### デスクリプション

挿入ソート（Insertion Sort）は、手持ちのトランプを並び替えるときに使われる、自然で思い付きやすいアルゴリズムの１つです。片手に持ったトランプを左から小さい順に並べる場合、１枚ずつカードを取り出して、それをその時点ですでにソートされている並びの適切な位置に挿入していくことによって、カードを並べ替えることができます。

挿入ソートは次のようなアルゴリズムになります

```c++
1 insertionSort(A, N) // N個の要素を含む0-オリジンの配列A
2   for i が 1 から N-1 まで
3     v = A[i]
4     j = i - 1
5     while j >= 0 かつ A[j] > v
6       A[j+1] = A[j]
7       j--
8     A[j+1] = v
```

*N* 個の要素を含む数列 *A* を昇順に並び替える挿入ソートのプログラムを作成してください。上の疑似コードに従いアルゴリズムを実装してください。アルゴリズムの動作を確認するため、各計算ステップでの配列（入力直後の並びと、各 *i* の処理が終了した直後の並び）を出力してください。

#### 入力

入力の最初の行に、数列の長さを表す整数 *N* が与えられます。2 行目に、*N* 個の整数が空白区切りで与えられます。

#### 出力

出力は *N* 行からなります。挿入ソートの各計算ステップでの途中結果を 1 行に出力してください。配列の要素は１つの空白で区切って出力してください。最後の要素の後の空白など、余計な空白や改行を含めると Presentation Error となりますので注意してください。

#### サンプル

入力

```c++
6
5 2 4 6 1 3
```

出力

```c++
5 2 4 6 1 3
2 5 4 6 1 3
2 4 5 6 1 3
2 4 5 6 1 3
1 2 4 5 6 3
1 2 3 4 5 6
```

### ソリューション

```c++
#include <iostream>
using namespace std;
void swap(int &a, int &b)
{
    int temp;
    temp=a;
    a=b;
    b=temp;
}
int* InsertionSort(int *A,int len)
{
    for(int i=1;i<len;i++)
    {
        for(int j=i;j>0;j--)
        {
            if(A[j]>A[j-1])//大于则已经到达正确位置
            {
                break;
            }
            else
            {
                swap(A[j],A[j-1]);//小于则还需要换位
            }
        }
    }
    return A;
}
int main() {
    int A[]={-1,-4,2,5,6,7,9,2,1,-10,-12,-17,-19};
    int len=sizeof(A)/sizeof(int);
    InsertionSort(A,len);
    for(int i=0;i<len;i++)
        cout<<A[i]<<' ';
    cout<<endl;
}
```

### 時間複雑度

**時間複雑度は$O(n^2)$である**

## 2.最大公約数

### デスクリプション

２つの自然数 *x*, *y* を入力とし、それらの最大公約数を求めるプログラムを作成してください。

２つの整数 *x* と *y* について、*x* ÷ *d* と *y* ÷ *d* の余りがともに 0 となる *d* のうち最大のものを、*x* と *y* の最大公約数（Greatest Common Divisor）と言います。例えば、35 と14 の最大公約数 gcd (35, 14) は 7 となります。これは、35 の約数{1, 5, 7, 35}、14 の約数 {1, 2, 7, 14} の公約数 {1, 7} の最大値となります。

#### 入力

*x* と *y* が１つの空白区切りで１行に与えられます

#### 出力

最大公約数を１行に出力してください。

#### サンプル

入力

```c++
147 105
```

出力

```c++
21
```

### ソリューション

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <cstring>
#include <string>
#include <math.h>
#include <stdlib.h>
using namespace std;
int GCD(int a,int b)//辗转相减
{
    while(a!=b)
    {
        if(a>b)
            a=a-b;
        else
            b=b-a;
    }
    return a;
}
int main()
{
    int a,b;
    cin>>a>>b;
    cout<<GCD(a,b)<<endl;
}
```

## 3.素数

### デスクリプション

約数が 1 とその数自身だけであるような自然数を素数と言います。例えば、最初の8 つの素数は2, 3, 5, 7, 11, 13, 17, 19 となります。1 は素数ではありません。

*n* 個の整数を読み込み、それらに含まれる素数の数を出力するプログラムを作成してください

#### 入力

最初の行に *n* が与えられます。続く *n* 行に *n* 個の整数が与えられます。

#### 出力

入力に含まれる素数の数を１行に出力してください。

### サンプル

入力

```c++
6
2
3
4
5
6
7
```

出力

```c++
4
```

### ソリューション

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <cstring>
#include <string>
#include <math.h>
#include <stdlib.h>
using namespace std;
bool isPrime(int n)
{
    for(int i=2;i*i<=n;i++)
    {
        if(n%i==0)
            return false;
    }
    return true;
}
int main()
{
    int count=0;
    int num,a;
    cin>>num;
    for(int i=0;i<num;i++)
    {
        cin>>a;
        if(isPrime(a))
            count++;
    }
    cout<<count<<endl;
}
```

## 4.最大の利益

### デスクリプション

FX取引では、異なる国の通貨を交換することで為替差の利益を得ることができます。例えば、１ドル100円の時に 1000ドル買い、価格変動により 1ドル 108円になった時に売ると、(108円 −− 100円) ×× 1000ドル == 8000円の利益を得ることができます。

ある通貨について、時刻 t における価格 Rt (t=0,1,2,,,n−1)が入力として与えられるので、価格の差 Rj−Ri (ただし、j>iとする) の最大値を求めてください。

#### 入力

最初の行に *n* が与えられます。続く *n* 行に *n* 個の整数が与えられます。

#### 出力

最大値を１行に出力してください

### サンプル

入力

```c++
6
5
3
1
3
4
3
```

出力

```c++
3
```

### ソリューション

```c++
#include <iostream>
#include <cstdio>
#include <limits.h>
#include <algorithm>
#include <vector>
#include <cstring>
#include <string>
#include <math.h>
#include <stdlib.h>
using namespace std;
int max_profit(int *value,int n)
{
    int minv=value[0];
    int maxv=INT_MIN;
    for(int i=1;i<n;i++)//每轮对比，找出最大和最小的差值
    {
        maxv=max(maxv,value[i]-minv);
        minv=min(minv,value[i]);
    }
    return maxv;
}
int main()
{
    int num;
    cin>>num;
    int *a=new int[num];
    for(int i=0;i<num;i++)
        cin>>a[i];
    cout<<max_profit(a,num)<<endl;
}
```

