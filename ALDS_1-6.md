# ALDS_1-6

## 1.計数ソート

### デスクリプション

計数ソートは各要素が 0 以上 $k$ 以下である要素数 $n$ の数列に対して線形時間$(O(n+k))$で動く安定なソーティングアルゴリズムです。

入力数列 $A$の各要素 $A_j$ について、$A_j$以下の要素の数をカウンタ配列 $C$ に記録し、その値を基に出力配列 $B$ における $A_j$の位置を求めます。同じ数の要素が複数ある場合を考慮して、要素 $A_j$ を出力（$B$ に入れる）した後にカウンタ $C[A_j]$ は修正する必要があります。詳しくは以下の疑似コードを参考にしてください。

```c++
1 CountingSort(A, B, k)
2     for i = 0 to k
3         C[i] = 0
4
5     /* C[i] に i の出現数を記録する */
6     for j = 1 to n
7         C[A[j]]++
8
9     /* C[i] に i 以下の数の出現数を記録する*/
10    for i = 1 to k
11        C[i] = C[i] + C[i-1]
12
13    for j = n downto 1
14        B[C[A[j]]] = A[j]
15        C[A[j]]--
```

数列 $A$ を読み込み、計数ソートのアルゴリズムで昇順に並び替え出力するプログラムを作成してください。上記疑似コードに従ってアルゴリズムを実装してください。

#### 入力

入力の最初の行に、数列 $A$の長さを表す整数 $n$ が与えられます。２行目に、$n$ 個の整数が空白区切りで与えられます。

#### 出力

整列された数列を1行に出力してください。数列の連続する要素は１つの空白で区切って出力してください

#### サンプル

入力

```
7
2 5 1 3 2 3 0
```

出力

```
0 1 2 2 3 3 5
```

### ソリューション

```c++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
#include <limits.h>
#include <sstream>
#include <algorithm>
#include <stdlib.h>
#include <unordered_set>
#include <cstring>
#include <queue>
#include <list>
#include <cstdio>
#include <math.h>
#include <map>
#include <set>
using namespace std;
void CountingSort(int* A, int* B, int n, int k)
{
	int* C = new int[k];
	int index = 0;
	for (int i = 0; i <= k; i++)
	{
			C[i] = 0;
	}
	for (int i = 0; i < n; i++)
	{
		C[A[i]]++;//计数
	}
	for (int j = 0; j <= k; j++)
	{	
		while (C[j])
		{
			B[index++] = j;
			C[j]--;//倒出所有元素
		}
	}
	for (int i = 0; i < index-1; i++)
		cout << B[i] << ' ';
	cout << B[index-1] << endl;
}
int main()
{
	int n;
	cin >> n;
	int* A = new int[n];
	int* B = new int[n];
	for (int i = 0; i < n; i++)
		cin >> A[i];
	int max_num = *max_element(A, A + n);
	int k = max_num;
	CountingSort(A, B, n, k);
}
```

### 時間複雑度

時間複雑度は$O(n+k)$である

## 2.Partition

### デスクリプション

partition ( A, p, r )は、配列 A[ p..r ] を A[ p..q − 1] の各要素が A[q] 以下で、A[ q +1.. r ] の各要素が A[ q ] より大きい A[ p..q − 1] と A[q + 1..r ] に分割し、インデックス q を戻り値として返します。

数列 $A$ を読み込み、次の疑似コードに基づいた partition を行うプログラムを作成してください。

```c++
1 partition(A, p, r)
2   x = A[r]
3   i = p-1
4   for j = p to r-1
4     if A[j] <= x
5       i = i+1
6       A[i] と A[j] を交換
7   A[i+1] と A[r] を交換
8   return i+1
```

ここで、$r$ は配列$ A$ の最後の要素を指す添え字で、$A[r]$ を基準として配列を分割することに注意してください。

#### 入力

入力の最初の行に、数列 $A$ の長さを表す整数 $n$が与えられます。２行目に、$n$ 個の整数 $A_i$ $(i=1,2,...,n)$ が空白区切りで与えられます

#### 出力

分割された数列を1行に出力してください。数列の連続する要素は１つの空白で区切って出力してください。また、partition の基準となる要素を [  ]で示してください

#### サンプル

入力

```
12
13 19 9 5 12 8 7 4 21 2 6 11
```

出力

```
12
13 19 9 5 12 8 7 4 21 2 6 11
```

### ソリューション

```C++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
#include <limits.h>
#include <sstream>
#include <algorithm>
#include <stdlib.h>
#include <unordered_set>
#include <cstring>
#include <queue>
#include <list>
#include <cstdio>
#include <math.h>
#include <map>
#include <set>
using namespace std;
//整体思路就是依次遍历数组，找到比x小的，i++，之后换掉，直到找不到比x小的，此时下标为i+1的是第一个比x大的数，将其与x互换
int partition(int* A, int p, int r)
{
	int x = A[r];
	int i = p - 1;
	for (int j = p; j < r; j++)
	{
		if (A[j] <= x)
		{
			i++;
			swap(A[i], A[j]);
		}
	}
	swap(A[i + 1], A[r]);
	return i + 1;
}
int main()
{
	int n;
	cin >> n;
	int* A = new int[n];
	for (int i = 0; i < n; i++)
	{
		cin >> A[i];
	}
	int q=partition(A, 0, n - 1);
	for (int i = 0; i < n; i++)
	{
		if (i)
			cout << ' ';
		if (i == q)
			cout << '[';
		cout<<A[i];
		if (i == q)
			cout << ']';
	}
	cout << endl;
}
```

### 時間複雑度

時間複雑度は$O(n)$である

## 3.クイックソート

### デスクリプション

$n$ 枚のカードの列を整列します。１枚のカードは絵柄(S, H, C, またはD)と数字のペアで構成されています。これらを以下の疑似コードに基づくクイックソートで数字に関して昇順に整列するプログラムを作成してください。partition は ALDS1_6_B の疑似コードに基づくものとします。

```c++
1 quicksort(A, p, r)
2   if p < r
3     q = partition(A, p, r)
4     quickSort(A, p, q-1)
5     quickSort(A, q+1, r)
```

#### 入力

1行目にカードの枚数 $n$ が与えられます。

2行目以降で $n$ 枚のカードが与えられます。各カードは絵柄を表す１つの文字と数（整数）のペアで１行に与えられます。絵柄と数は１つの空白で区切られています

#### 出力

1行目に、この出力が安定か否か（StableまたはNot stable）を出力してください。

2行目以降で、入力と同様の形式で整列されたカードを順番に出力してください（$n$ を出力する必要はありません）。

#### サンプル

入力

```
6
D 3
H 2
D 1
S 3
D 2
C 1
```

出力

```
Not stable
D 1
C 1
D 2
H 2
D 3
S 3
```

### ソリューション

```c++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
#include <limits.h>
#include <sstream>
#include <algorithm>
#include <stdlib.h>
#include <unordered_set>
#include <cstring>
#include <queue>
#include <list>
#include <cstdio>
#include <math.h>
#include <map>
#include <set>
using namespace std;
//偷懒先用的冒泡直接O(n^2)超时了，然后就改归并了orz，快排也可以像归并这么写，但是反正AC了懒得改了orz
void merge(pair<char, int>* p,int left, int mid, int right)
{
    //归并的思路就是挨个比，把小的先往结果数组里放就完事了
	int n1 = mid - left;
	int n2 = right - mid;
	pair<char, int>* L = new pair<char, int>[n1 + 1];
	pair<char, int>* R = new pair<char, int>[n2 + 1];
	for (int i = 0; i < n1; i++)
		L[i] = p[left + i];
	for (int i = 0; i < n2; i++)
		R[i] = p[mid + i];
	L[n1].second = R[n2].second = INT_MAX;
	int i = 0, j = 0;
	for (int k = left; k < right; k++)
	{
		if (L[i].second <= R[j].second)
		{
			p[k] = L[i];
			i++;
		}
		else
		{
			p[k] = R[j];
			j++;
		}
	}
}
void mergesort(pair<char, int>* p, int left, int right)
{
	if (right - left > 1)
	{
		int mid = (left + right) / 2;
		mergesort(p, left, mid);
		mergesort(p, mid, right);
		merge(p, left, mid, right);
	}
}
int partition(int* A, int p, int r, pair<char, int>* pa)
{
	int x = A[r];
	int i = p - 1;
	for (int j = p; j < r; j++)
	{
		if (A[j] <= x)
		{
			i++;
			swap(A[i], A[j]);
			swap(pa[i], pa[j]);
		}
	}
	swap(A[i + 1], A[r]);
	swap(pa[i + 1], pa[r]);
	return i + 1;
}
void quicksort(int* A, int p, int r, pair<char, int>* pa)
{
	if (p < r)
	{
		int q = partition(A, p, r, pa);
		quicksort(A, p, q - 1, pa);
		quicksort(A, q + 1, r, pa);
	}
}
int main()
{
	int n;
	cin >> n;
	pair<char, int>* p1 = new pair<char, int>[n];
	pair<char, int>* p2 = new pair<char, int>[n];
	int* A = new int[n];
	int* B = new int[n];
	for (int i = 0; i < n; i++)
	{
		cin >> p1[i].first >> p1[i].second;
	}
	for (int i = 0; i < n; i++)
	{
		p2[i].first = p1[i].first;
		p2[i].second = p1[i].second;
		A[i] = p1[i].second;
		B[i] = A[i];
	}
	mergesort(p1, 0, n);
	quicksort(B, 0, n - 1, p2);
	int i;
	for (i = 0; i < n; i++)
	{
		if (p1[i].first != p2[i].first)
		{
			cout << "Not stable" << endl;
			break;
		}
	}
	if (i == n)
		cout << "Stable" << endl;
	for (int j = 0; j < n; j++)
	{
		cout << p2[j].first << ' ' << p2[j].second << endl;
	}
}
```

すべての操作の時間複雑度はO(1)である

## 4.Minimum Cost Sort(?)

### デスクリプション

重さ $w_i(i=0,1,...,n−1)$の $n$ 個の荷物が１列に並んでいます。これらの荷物をロボットアームを用いて並べ替えます。１度の操作でロボットアームは荷物 $i$ と荷物 $j$ を持ち上げ、それらの位置を交換することができますが、$w_i+w_j$のコストがかかります。ロボットアームは何度でも操作することができます。

与えられた荷物の列を重さの昇順に整列するコストの総和の最小値を求めてください。

#### 入力

１行目に整数 $n$ が与えられます。２行目に $n$ 個の整数 $w_i(i=0,1,2,...n−1)$ が空白区切りで与えられます。

#### 出力

最小値を１行に出力してください。

#### サンプル

入力

```
5
1 5 3 4 2
```

出力

```
7
```

### ソリューション

```c++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
#include <limits.h>
#include <sstream>
#include <algorithm>
#include <stdlib.h>
#include <unordered_set>
#include <cstring>
#include <queue>
#include <list>
#include <cstdio>
#include <math.h>
#include <map>
#include <set>
#define MAX 1000
#define VMAX 10000
int n, A[MAX], s;
int B[MAX], T[VMAX + 1];
using namespace std;
int solution()
{
	int ans = 0;
	bool V[MAX];
	for (int i = 0; i < n; i++)
	{
		B[i] = A[i];
		//i位置没有使用过
		V[i] = false;
	}
	sort(B, B + n);
	for (int i = 0; i < n; i++)
		T[B[i]] = i;
	for (int i = 0; i < n; i++)
	{
		if (V[i])//V[i]是否被使用
			continue;
		int curr = i;
		int circle_sum = 0;
		int minimal = VMAX;
		int circle_len = 0;
		while (true)
		{
			V[curr] = true;
			circle_len++;
			int v = A[curr];
			minimal = min(minimal, v);//记录圈内最小
			circle_sum += v;//放入圈内的数在排好序之后的位置
			curr = T[v];
			if (V[curr])//如果位置被使用
				break;//封闭结束
		}
		ans += min(circle_sum + (circle_len - 2) * minimal, minimal +circle_sum +(circle_len + 1) * s);
	}
	return ans;
}
int main()
{
	cin >> n;
	s = VMAX;
	for (int i = 0; i < n; i++)
	{
		cin >> A[i];
		s = min(s, A[i]);
	}
	int ans = solution();
	cout << ans << endl;
}
```