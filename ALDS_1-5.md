# ALDS_1-5

## 1.総当たり

### デスクリプション

長さ $n$ の数列 $A$ と整数 $m$ に対して、$A$の要素の中のいくつかの要素を足しあわせて $m$ が作れるかどうかを判定するプログラムを作成してください。$A$ の各要素は１度だけ使うことができます。

数列 $A$ が与えられたうえで、質問として $q$ 個の $m_i$ が与えれるので、それぞれについて "yes" または "no" と出力してください。

#### 入力

１行目に $n$、２行目に $A$ を表す $n$ 個の整数、３行目に $q$、４行目に $q$ 個の整数 $m_i$が与えられます

#### 出力

各質問について $A$ の要素を足しあわせて $m_i$ を作ることができれば yes と、できなければ no と出力してください。

#### サンプル

入力

```
5
1 5 7 10 21
4
2 4 17 8
```

出力

```
no
no
yes
yes
```

### アイデア

この問題で、配列Aの要素$A[i]$を取る時は一つの状況、取らない時は別の一つの状況である。ゆえに、$2^n$種類の状況がある。その時、再帰を用いて、結果の中で$A[i]$ が含まれるか否かも考えなければならない。

### ソリューション

```c++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
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
int n, A[100];
bool solution(int i, int target)//i为下标，target为加和的数
{
	if (target == 0)
		return 1;
	if (i >= n)//i大于A的size
		return 0;
	int res = solution(i + 1, target) || solution(i + 1, target - A[i]);
	return res;
}
int main()
{
	int target, m;
	cin >> n;
	for (int i = 0; i < n; i++)
		cin >> A[i];
	cin >> m;
	for (int i = 0; i < m; i++)
	{
		cin>>target;
		if (solution(0, target))
			cout << "yes" << endl;
		else
			cout << "no" << endl;
	}
}
```

### 時間複雑度

時間複雑度は$O(2^n)$である

## 2.マージソート

### デスクリプション

マージソート（Merge Sort）は分割統治法に基づく高速なアルゴリズムで、次のように実装することができます。

```c++
merge(A, left, mid, right)
  n1 = mid - left;
  n2 = right - mid;
  L[0...n1], R[0...n2] を生成
  for i = 0 to n1-1
    L[i] = A[left + i]
  for i = 0 to n2-1
    R[i] = A[mid + i]
  L[n1] = INFTY
  R[n2] = INFTY
  i = 0
  j = 0
  for k = left to right-1
    if L[i] <= R[j]
      A[k] = L[i]
      i = i + 1
    else 
      A[k] = R[j]
      j = j + 1

mergeSort(A, left, right)
  if left+1 < right
    mid = (left + right)/2;
    mergeSort(A, left, mid)
    mergeSort(A, mid, right)
    merge(A, left, mid, right)
```

#### 入力

１行目に $n$、２行目に $S$ を表す $n$ 個の整数が与えられます。

#### 出力

１行目に整列済みの数列 $S$ を出力してください。数列の隣り合う要素は１つの空白で区切ってください。２行目に比較回数を出力してください

#### サンプル

入力

```
10
8 5 9 2 6 3 7 1 10 4
```

出力

```
1 2 3 4 5 6 7 8 9 10
34
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
int cnt = 0;
void merge(int* A, int left, int mid, int right)
{
	int n1 = mid - left;
	int n2 = right - mid;
	int* L = new int[n1+1];
	int* R = new int[n2+1];
	for (int i = 0; i < n1; i++)
		L[i] = A[left + i];
	for (int i = 0; i < n2; i++)
		R[i] = A[mid + i];
	L[n1] = INT_MAX;
	R[n2] = INT_MAX;
	int i = 0, j = 0;
	for (int k = left; k < right; k++)
	{
		if (L[i] <= R[j])
		{
			A[k] = L[i];
			i++;
		}
		else
		{
			A[k] = R[j];
			j++;
		}
		cnt++;
	}
}
void merge_Sort(int* A, int left, int right)
{
	if (right - left > 1)
	{
		int mid = (left + right) / 2;
		merge_Sort(A, left, mid);
		merge_Sort(A, mid, right);
		merge(A, left, mid, right);
	}
}
int main()
{
	int n;
	cin >> n;
	int* A = new int[n];
	for (int i = 0; i < n; i++)
		cin >> A[i];
	merge_Sort(A, 0, n);
	cout << A[0];
	for (int i = 1; i < n; i++)
		cout << ' '<<A[i];
	cout << endl;
	cout << cnt << endl;
}
```

### 時間複雑度

時間複雑度は$O(nlogn)$である

## 3.コッホ曲線

### デスクリプション

整数 $n$を入力し、深さ $n$ の再帰呼び出しによって作成されるコッホ曲線の頂点の座標を出力するプログラムを作成してください。

コッホ曲線は[フラクタル](http://en.wikipedia.org/wiki/Fractal)の一種として知られています。フラクタルとは再帰的な構造を持つ図形のことで、以下のように再帰的な関数の呼び出しを用いて描画することができます。

- 与えられた線分 $(p1,p2)$ を 3 等分する。
- 線分を 3等分する 2 点 $s,t$ を頂点とする正三角形 $(s,u,t)$ を作成する。
- 線分 $(p1,s)$、線分 $(s,u)$、線分 $(u,t)$、線分 $(t,p2)$に対して再帰的に同じ操作を繰り返す。

![image-20230306185214207](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230306185214207.png)

#### 入力

1 つの整数 $n$ が与えられます。

#### 出力

コッホ曲線の各頂点の座標 $(x,y)$を出力してください。１行に１点の座標を出力してください。端点の１つ $(0,0)$ から開始し、一方の端点 $(100,0)$ で終えるひと続きの線分の列となる順番に出力してください。出力は 0.0001 以下の誤差を含んでいてもよいものとします

#### サンプル

入力

```
1
```

出力

```
0.00000000 0.00000000
33.33333333 0.00000000
50.00000000 28.86751346
66.66666667 0.00000000
100.00000000 0.00000000
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
struct point
{
	double x;
	double y;
};
void koch(int m, point p1, point p2)
{
	if (m == 0)
		return;
	point s, t, u;
	double theta = M_PI * 60.0 / 180.0;
	s.x = p1.x + (p2.x - p1.x) / 3.0;
	s.y = p1.y + (p2.y - p1.y) / 3.0;
	t.x = p1.x + (p2.x - p1.x) * 2.0 / 3.0;
	t.y = p1.y + (p2.y - p1.y) * 2.0 / 3.0;
	u.x = (t.x - s.x) * cos(theta) - (t.y - s.y) * sin(theta) + s.x;//旋转矩阵
	u.y = (t.x - s.x) * sin(theta) + (t.y - s.y) * cos(theta) + s.y;//旋转矩阵
	koch(m - 1, p1, s);//递归画三角
	printf("%.8f %.8f\n", s.x, s.y);
	koch(m - 1, s, u);
	printf("%.8f %.8f\n", u.x, u.y);
	koch(m - 1, u, t);
	printf("%.8f %.8f\n", t.x, t.y);
	koch(m - 1, t, p2);
}
int main()
{
	int n;
	cin >> n;
	point p1, p2;
	p1.x = 0;
	p2.x = 100;
	p1.y = 0;
	p2.y = 0;
	printf("%.8f %.8f\n", p1.x, p1.y);
	koch(n, p1, p2);
	printf("%.8f %.8f\n", p2.x, p2.y);
}
```

すべての操作の時間複雑度はO(1)である

## 4.割り当て

### デスクリプション

数列 $A={a0,a1,...an−1}={0,1,...a_{n−1}}$ について、$a_i>a_j$かつ $i<j$ である組 $(i,j)$ の個数を反転数と言います。反転数は次のバブルソートの交換回数と等しくなります

```c++
bubbleSort(A)
  cnt = 0 // 反転数
  for i = 0 to A.length-1
    for j = A.length-1 downto i+1
      if A[j] < A[j-1]
	swap(A[j], A[j-1])
	cnt++

  return cnt
```

数列 $A$ が与えられるので、$A$ の反転数を求めてください。上の疑似コードのアルゴリズムをそのまま実装するとTime Limit Exceeded になることに注意してください。

#### 入力

１行目に数列 $A$ の長さ $n$ が与えられます。２行目に $a_i(i=0,1,..n−1)$ が空白区切りで与えられます

#### 出力

反転数を１行に出力してください。

#### サンプル

入力

```
5
3 5 2 1 4
```

出力

```
6
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
long long int cnt = 0;
void merge(int* A, int left, int mid, int right)
{
	int n1 = mid - left;
	int n2 = right - mid;
	int* L = new int[n1+1];
	int* R = new int[n2+1];
	for (int i = 0; i < n1; i++)
		L[i] = A[left + i];
	for (int i = 0; i < n2; i++)
		R[i] = A[mid + i];
	L[n1] = INT_MAX;
	R[n2] = INT_MAX;
	int i = 0, j = 0;
	for (int k = left; k < right; k++)
	{
		if (L[i] < R[j])
		{
			A[k] = L[i];
			i++;
		}
		else
		{
			A[k] = R[j];
			j++;
			cnt += n1 - i;//L中有多少个元素会移动到R[j]后方，因为i的次数是L中已经装入A的元素数，因此剩下的就是截至到目前元素的逆序数，加和即为逆序数
		}
	}
}
void merge_Sort(int* A, int left, int right)
{
	if (right - left > 1)
	{
		int mid = (left + right) / 2;
		merge_Sort(A, left, mid);
		merge_Sort(A, mid, right);
		merge(A, left, mid, right);
	}
}
int main()
{
	int n;
	cin >> n;
	int* A = new int[n];
	for (int i = 0; i < n; i++)
		cin >> A[i];
	merge_Sort(A, 0, n);
	cout << cnt << endl;
}
```