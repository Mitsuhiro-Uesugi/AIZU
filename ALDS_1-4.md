# ALDS_1-4

## 1.線形探索

### デスクリプション

$n$ 個の整数を含む数列 $S$ と、$q$ 個の異なる整数を含む数列 $T$ を読み込み、$T$ に含まれる整数の中で $S$ に含まれるものの個数 $C$ を出力するプログラムを作成してください。

#### 入力

１行目に $n$、２行目に $S$ を表す $n$ 個の整数、３行目に $q$、４行目に $T$ を表す $q$ 個の整数が与えられます

#### 出力

$C$ を１行に出力してください

#### サンプル

入力

```
5
1 2 3 4 5
3
3 4 1
```

出力

```
3
```

### ソリューション

```c++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
#include <sstream>
#include <algorithm>
#include <stdlib.h>
#include <cstring>
#include <queue>
#include <list>
#include <cstdio>
#include <math.h>
#include <map>
using namespace std;
int main()
{
	int n1, n2, temp, count = 0;;
	cin >> n1;
	int* A = new int[n1];
	for (int i = 0; i < n1; i++)
	{
		cin >> A[i];
	}
	cin >> n2;
	for (int i = 0; i < n2; i++)
	{
		cin >> temp;
		for (int j = 0; j < n1; j++)
		{
			if (A[j] == temp)
			{
				count++;
				break;//原题问的是T中的数在S里出现了几个，所以一旦出现了就要break
			}
		}
	}
	cout << count << endl;
}
```

### 時間複雑度

時間複雑度は$O(n^2)$である

## 2.二分探索

### デスクリプション

$n$ 個の整数を含む数列 $S$ と、$q$ 個の異なる整数を含む数列 $T$ を読み込み、$T$ に含まれる整数の中で $S$ に含まれるものの個数 $C$ を出力するプログラムを作成してください。

#### 入力

１行目に $n$、２行目に $S$ を表す $n$ 個の整数、３行目に $q$、４行目に $T$ を表す $q$ 個の整数が与えられます

#### 出力

$C$ を１行に出力してください

#### サンプル

入力

```
5
1 2 3 4 5
3
3 4 1
```

出力

```
3
```

### ソリューション

```c++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
#include <sstream>
#include <algorithm>
#include <stdlib.h>
#include <cstring>
#include <queue>
#include <list>
#include <cstdio>
#include <math.h>
#include <map>
using namespace std;
bool BinarySearch(int* A, int e,int n)
{
	int left = 0, right = n - 1;
	while (left <= right)
	{
		if (e == A[left] || e == A[right])//边界判断
			return true;
		else if (e < A[left + (right - left) / 2])//注意这里是right-left
			right = left + (right - left) / 2 - 1;
		else if (e > A[left + (right - left) / 2])
			left = left + (right - left) / 2 + 1;
		else//正好卡在中点
			return true;
	}
	return false;
}
int main()
{
	int n1, n2, temp, count = 0;
	cin >> n1;
	int* A = new int[n1];
	for (int i = 0; i < n1; i++)
	{
		cin >> A[i];
	}
	cin >> n2;
	for (int i = 0; i < n2; i++)
	{
		cin >> temp;
		if (BinarySearch(A,temp,n1))
		{
			count++;
		}
	}
	cout << count << endl;
 }
```

### 時間複雑度

時間複雑度は$O(nlogn)$である

## 3.辞書

### デスクリプション

以下の命令を実行する簡易的な「辞書」を実装してください。

- insert $str$: 辞書に $str$ を追加する。
- find $str$: 辞書に $str$ が含まれる場合 'yes'と、含まれない場合 'no'と出力する。

#### 入力

１行目に $n$、２行目に $S$ を表す $n$ 個の整数、３行目に $q$、４行目に $T$ を表す $q$ 個の整数が与えられます

#### 出力

$C$ を１行に出力してください

#### サンプル

入力

```c++
13
insert AAA
insert AAC
insert AGA
insert AGG
insert TTT
find AAA
find CCC
find CCC
insert CCC
find CCC
insert T
find TTT
find T
```

出力

```c++
yes
no
no
yes
yes
yes
```

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
int main()
{
	int n;
	pair<string, string> p;
	unordered_set<string> us;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> p.first >> p.second;
		if (p.first == "insert")
			us.insert(p.second);
		else if (p.first == "find")
		{
			if (us.find(p.second) != us.end())
				cout << "yes" << endl;
			else
				cout << "no" << endl;
		}
	}
}
```

すべての操作の時間複雑度は$O(1)$である

## 4.割り当て

### デスクリプション

重さがそれぞれ$ w_i(i=0,1,...n−1)$ の $n$個の荷物が、ベルトコンベアから順番に流れてきます。これらの荷物を $k$ 台のトラックに積みます。各トラックには連続する 0 個以上の荷物を積むことができますが、それらの重さの和がトラックの最大積載量 $P$ を超えてはなりません。最大積載量 $P$ はすべてのトラックで共通です。

$n$、$k$、$w_i$ が与えられるので、すべての荷物を積むために必要な最大積載量 $P$ の最小値を求めるプログラムを作成してください。い。

#### 入力

最初の行に荷物の数 $n$ とトラックの数 $k$ が空白区切りで与えられます。続く $n$ 行に $n$ 個の整数 $w_i$ がそれぞれ１行に与えられます。

#### 出力

$P$の最小値を１行に出力してください

#### サンプル

入力

```c++
5 3
8
1
7
3
9
```

出力

```c++
10
```

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
#define MAX 100000
#define MAXP 100000 * 10000
using namespace std;
int n, k;//货物数和车数
int A[MAX];
int check(long long p)
{
	int i = 0;
	for (int j = 0; j < k; j++)
	{
		long long s = 0;
		while (s + A[i] <= p)
		{
			s += A[i];
			i++;
			if (i == n)//装满了所有的货物还没有到p，需要收缩边界
				return n;
		}
	}
	return i;
}
int solution()
{
	long long left = 0;
	long long right = MAXP;
	long long mid;
	while (right - left > 1)//<=1则找到了正确的p
	{
		mid = (left + right) / 2;
		int val = check(mid);
		if (val >= n)
			right = mid;
		else
			left = mid;
	}
	return right;
}
int main()
{
	cin >> n >> k;
	for (int i = 0; i < n; i++)
		cin >> A[i];
	long long result = solution();
	cout << result << endl;
}
```

