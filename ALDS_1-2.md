# ALDS1-2

## 1.バブルソート

### デスクリプション

バブルソートはその名前が表すように、泡（Bubble）が水面に上がっていくように配列の要素が動いていきます。バブルソートは次のようなアルゴリズムで数列を昇順に並び変えます

```c++
1 bubbleSort(A, N) // N 個の要素を含む 0-オリジンの配列 A
2   flag = 1 // 逆の隣接要素が存在する
3   while flag
4     flag = 0
5     for j が N-1 から 1 まで
6       if A[j] < A[j-1]
7         A[j] と A[j-1] を交換
8         flag = 1
```

数列 *A* を読み込み、バブルソートで昇順に並び変え出力するプログラムを作成してください。また、バブルソートで行われた要素の交換回数も報告してください

#### 入力

入力の最初の行に、数列の長さを表す整数 *N* が与えられます。２行目に、*N* 個の整数が空白区切りで与えられます。

#### 出力

出力は 2 行からなります。１行目に整列された数列を 1 行に出力してください。数列の連続する要素は１つの空白で区切って出力してください。2 行目に交換回数を出力してください。

#### サンプル

入力

```c++
5
5 3 2 4 1
```

出力

```c++
1 2 3 4 5
8
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
int* Bubble_Sort(int* A,int n)
{
    int count=0;
    for(int i=0;i<n;i++)
    {
        int flag;
        for(int j=0;j<n-i-1;j++)
        {
            if(A[j]>A[j+1])
            {
                swap(A[j], A[j + 1]);
                count++;
                flag = 1;
            }
        }
        if(flag==0)
            break;
    }
    for(int i=0;i<n;i++)
    {
        if(i==n-1)
            cout<<A[i]<<endl;
        else
            cout << A[i] << ' ';
    }
    cout<<count<<endl;
}

int main()
{
    int n;
    cin>>n;
    int *A=new int [n];
    for(int i=0;i<n;i++)
        cin>>A[i];
    Bubble_Sort(A,n);
}
```

### 時間複雑度

**時間複雑度は$O(n^2)$である**

### 選択ソート

選択ソートは、各計算ステップで１つの最小値を「選択」していく、直観的なアルゴリズムです。

```c++
1 selectionSort(A, N) // N個の要素を含む0-オリジンの配列A
2   for i が 0 から N-1 まで
3     minj = i
4     for j が i から N-1 まで
5       if A[j] < A[minj]
6         minj = j
7     A[i] と A[minj] を交換
```

数列Aを読み込み、選択ソートのアルゴリズムで昇順に並び替え出力するプログラムを作成してください。上の疑似コードに従いアルゴリズムを実装してください。

疑似コード 7 行目で、*i* と *minj* が異なり実際に交換が行われた回数も出力してください。

#### 入力

入力の最初の行に、数列の長さを表す整数 *N* が与えられます。２行目に、*N* 個の整数が空白区切りで与えられます。

#### 出力

出力は 2 行からなります。１行目に整列された数列を 1 行に出力してください。数列の連続する要素は１つの空白で区切って出力してください。2 行目に交換回数を出力してください。

#### サンプル

入力

```c++
6
5 6 4 2 1 3
```

出力

```c++
1 2 3 4 5 6
4
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
int* Selection_Sort(int* A,int n)//从无序区找一个最小的，作为有序区的下一个元素
{
    int count=0;
    for(int i=0;i<n;i++)
    {
        int index=i;
        for(int j=i;j<n;j++)
        {
            if(A[j]<A[index])
                index=j;
        }
        swap(A[i],A[index]);
        if(index!=i)
            count++;
    }
    for(int i=0;i<n-1;i++)
        cout<<A[i]<<' ';
    cout<<A[n-1]<<endl;
    cout<<count<<endl;
    return A;
}

int main()
{
    int n;
    cin>>n;
    int *A=new int [n];
    for(int i=0;i<n;i++)
        cin>>A[i];
    Selection_Sort(A,n);
}
```

## 3.安定なソート

### デスクリプション

トランプのカードを整列しましょう。ここでは、４つの絵柄(S, H, C, D)と９つの数字(1, 2, ..., 9)から構成される計 36 枚のカードを用います。例えば、ハートの 8 は"H8"、ダイヤの 1 は"D1"と表します。

バブルソート及び選択ソートのアルゴリズムを用いて、与えられた *N* 枚のカードをそれらの数字を基準に昇順に整列するプログラムを作成してください。アルゴリズムはそれぞれ以下に示す疑似コードに従うものとします。数列の要素は 0 オリジンで記述されています。

```c++
1 BubbleSort(C, N)
2   for i = 0 to N-1
3     for j = N-1 downto i+1
4       if C[j].value < C[j-1].value
5         C[j] と C[j-1] を交換
6
7 SelectionSort(C, N)
8   for i = 0 to N-1
9     minj = i
10    for j = i to N-1
11      if C[j].value < C[minj].value
12        minj = j
13    C[i] と C[minj] を交換
```

また、各アルゴリズムについて、与えられた入力に対して安定な出力を行っているか報告してください。ここでは、同じ数字を持つカードが複数ある場合それらが入力に出現する順序で出力されることを、「安定な出力」と呼ぶことにします。（※常に安定な出力を行うソートのアルゴリズムを安定なソートアルゴリズムと言います。）

#### 入力

1 行目にカードの枚数 *N* が与えられます。 2 行目に *N* 枚のカードが与えられます。各カードは絵柄と数字のペアを表す２文字であり、隣合うカードは１つの空白で区切られています

#### 出力

1 行目に、バブルソートによって整列されたカードを順番に出力してください。隣合うカードは１つの空白で区切ってください。
2 行目に、この出力が安定か否か（Stable またはNot stable）を出力してください。
3 行目に、選択ソートによって整列されたカードを順番に出力してください。隣合うカードは１つの空白で区切ってください。
4 行目に、この出力が安定か否か（Stable またはNot stable）を出力してください。

### サンプル

入力

```c++
D2 C3 H4 S4 C9
Stable
D2 C3 S4 H4 C9
Not stable
```

出力

```c++
D2 C3 H4 S4 C9
Stable
D2 C3 S4 H4 C9
Not stable
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
struct poker
{
    char mark;
    int num;
};
void Selection_Sort(poker* A,int n)
{
    for(int i=0;i<n;i++)
    {
        int index=i;
        for(int j=i;j<n;j++)
        {
            if(A[j].num<A[index].num)
                index=j;
        }
        swap(A[i],A[index]);
    }
    for(int i=0;i<n-1;i++)
        cout<<A[i].mark<<A[i].num<<' ';
    cout<<A[n-1].mark<<A[n-1].num<<endl;
}
void Bubble_Sort(poker* A,int n)
{
    for(int i=0;i<n;i++)
    {
        for(int j=n-1;j>=i+1;j--)
        {
            if(A[j].num<A[j-1].num)
                swap(A[j],A[j-1]);
        }
    }
    for(int i=0;i<n-1;i++)
        cout<<A[i].mark<<A[i].num<<' ';
    cout<<A[n-1].mark<<A[n-1].num<<endl;
}
int main()
{
    int n;
    cin>>n;
    poker *Select=new poker[n];
    poker *bubble=new poker[n];
    char buffer[3];
    for(int i=0;i<n;i++)
    {
        cin>>buffer;
        Select[i].mark=buffer[0];
        Select[i].num=buffer[1]-'0';
        bubble[i]=Select[i];
    }
    Bubble_Sort(bubble,n);//冒泡一定是稳定排序，所以选择排序和冒泡一样就稳定，不一样就不稳定
    cout<<"Stable"<<endl;
    Selection_Sort(Select,n);
    int j;
    for(j=0;j<n;j++)
        if(bubble[j].mark!=Select[j].mark||bubble[j].num!=Select[j].num)
            break;
    if(j!=n)
        cout<<"Not stable"<<endl;
    else
        cout<<"Stable"<<endl;
}
```

## 4.Shell Sort

### デスクリプション

次のプログラムは、挿入ソートを応用して $n$ 個の整数を含む数列 $A$ を昇順に整列するプログラムです。

```c++
1  insertionSort(A, n, g)
2      for i = g to n-1
3          v = A[i]
4          j = i - g
5          while j >= 0 && A[j] > v
6              A[j+g] = A[j]
7              j = j - g
8              cnt++
9          A[j+g] = v
10
11 shellSort(A, n)
12     cnt = 0
13     m = ?
14     G[] = {?, ?,..., ?}
15     for i = 0 to m-1
16         insertionSort(A, n, G[i])
```

数列Aを読み込み、選択ソートのアルゴリズムで昇順に並び替え出力するプログラムを作成してください。上の疑似コードに従いアルゴリズムを実装してください。

疑似コード 7 行目で、*i* と *minj* が異なり実際に交換が行われた回数も出力してください。

#### 入力

入力の最初の行に、数列の長さを表す整数 *N* が与えられます。２行目に、*N* 個の整数が空白区切りで与えられます。

#### 出力

出力は 2 行からなります。１行目に整列された数列を 1 行に出力してください。数列の連続する要素は１つの空白で区切って出力してください。2 行目に交換回数を出力してください。

#### サンプル

入力

```c++
6
5 6 4 2 1 3
```

出力

```c++
1 2 3 4 5 6
4
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
#include <vector>
#include <math.h>
#include <stdlib.h>
using namespace std;

long long cnt;
int l;
int A[1000000];
int n;
vector<int> G;

//插入排序，以g为间隔分组，不满足顺序则连续交换直到满足
void insertionSort(int A[], int n , int g){
    for(int i = g; i < n; i++){
        int v = A[i];
        int j = i-g;
        while(j >= 0 && A[j] > v){
            A[j+g] = A[j];
            j -= g;
            cnt++;
        }
        A[j+g] = v;
    }
}
/*统计count(交换次数)*/
void shellSort(int A[], int n){
    int h = 1;
    while(h <= n){
        G.push_back(h);
        h = 3*h + 1;
    }

    for(int i = G.size()-1; i >= 0; i--){
        insertionSort(A, n, G[i]);//以不同的间隔进行排序
    }
}

int main(){
    scanf("%d" , &n);
    for(int i = 0; i < n; i++){
        scanf("%d" , &A[i]);
    }

    cnt = 0;
    shellSort(A,n);

    printf("%d\n", G.size());
    for(int i = G.size()-1; i >= 0; i--){
        printf("%d ",G[i]);
    }

    printf("\n%d\n", cnt);

    for(int i = 0; i < n; i++){
        printf("%d\n", A[i]);
    }

    return 0;
}
```

### 時間複雑度

**最悪時間複雑度は$O(n^2)$である**

**最優時間複雑度は$O(n)$である**

**平均時間複雑度は$O(n^{1.3})$である**

