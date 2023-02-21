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

