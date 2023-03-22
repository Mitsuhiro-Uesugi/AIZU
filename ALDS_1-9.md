# ALDS_1-9

## 完全二分木

### デスクリプション

すべての葉が同じ深さを持ち、すべての内部節点の次数が 2 であるような二分木を完全二分木と呼びます。また、二分木の最下位レベル以外のすべてのレベルは完全に埋まっており、最下位レベルは最後の節点まで左から順に埋まっているような木も（おおよそ）完全二分木と呼びます。

二分ヒープは、次の図のように、木の各節点に割り当てられたキーが１つの配列の各要素に対応した完全二分木で表されたデータ構造です。

![image-20230322134623960](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230322134623960.png)

二分ヒープを表す配列を $A$、二分ヒープのサイズ（要素数）を $H$ とすれば、$A[1...H]$に二分ヒープの要素が格納されます。木の根の添え字は $1$ であり、節点の添え字 $i$ が与えられたとき、その親 $parent(i)$、左の子 $left(i)$、右の子 $right(i)$ はそれぞれ $⌊i/2⌋$、$2×i$、$2×i+1$で簡単に算出することができます。

完全二分木で表された二分ヒープを読み込み、以下の形式で二分ヒープの各節点の情報を出力するプログラムを作成してください。

node $id$: key = $k$, parent key = $pk$, left key = $lk$, right key = $rk$

ここで、$id$ は節点の番号（インデックス）、$k$ は節点の値、$pk$ は親の値、$lk$ は左の子の値、$rk$ は右の子の値を示します。これらの情報をこの順番で出力してください。ただし、該当する節点が存在しない場合は、出力を行わないものとします

#### 入力

入力の最初の行に、二分ヒープのサイズ $H$ が与えられます。続いて、ヒープの節点の値（キー）を表す $H$ 個の整数がそれらの節点の番号順に空白区切りで与えられます

#### 出力

上記形式で二分ヒープの節点の情報をインデックスが 1 から $H$ に向かって出力してください。各行の最後が空白となることに注意してください。

#### サンプル

入力

```
5
7 8 1 2 3
```

出力

```
node 1: key = 7, left key = 8, right key = 1, 
node 2: key = 8, parent key = 7, left key = 2, right key = 3, 
node 3: key = 1, parent key = 7, 
node 4: key = 2, parent key = 8, 
node 5: key = 3, parent key = 8, 
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
	cin >> n;
	int* Heap = new int[n+1];
	for (int i = 1; i <= n; i++)
		cin >> Heap[i];
	for (int i = 1; i <= n; i++)
	{
		cout << "node " << i <<": key = " << Heap[i] << ", ";
		if (i / 2 != 0)
			cout << "parent key = " << Heap[i / 2] << ", ";
		if (2 * i <= n)
			cout << "left key = " << Heap[2 * i] << ", ";
		if (2 * i + 1 <= n)
			cout << "right key = " << Heap[2 * i + 1] << ", ";
		cout << endl;
	}
	return 0;
}
```

## 2.最大ヒープ

### デスクリプション

「節点のキーがその親のキー以下である」という max-ヒープ条件を満たすヒープを、max-ヒープと呼びます。max-ヒープでは、最大の要素が根に格納され、ある節点を根とする部分木の節点のキーは、その部分木の根のキー以下となります。親子間のみに大小関係があり、兄弟間に制約はないことに注意してください。

例えば、下図はmax-ヒープの例です。

![image-20230322153050178](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230322153050178.png)

$maxHeapify(A,i)$は、節点 $i$ を根とする部分木が max-ヒープになるよう $A[i]$の値を max-ヒープの葉へ向かって下降させます。ここで $H$ をヒープサイズとします

```c++
1  maxHeapify(A, i)
2      l = left(i)
3      r = right(i)
4      // 左の子、自分、右の子で値が最大のノードを選ぶ
5      if l ≤ H and A[l] > A[i]
6          largest = l
7      else 
8          largest = i
9      if r ≤ H and A[r] > A[largest]
10         largest = r
11
12     if largest ≠ i　// i の子の方が値が大きい場合
13         A[i] と A[largest] を交換
14         maxHeapify(A, largest) // 再帰的に呼び出し
```

次の buildMaxHeap(A) はボトムアップに maxHeapify を適用することで配列 $A$ を max-ヒープに変換します。

```c++
1 buildMaxHeap(A)
2    for i = H/2 downto 1
3        maxHeapify(A, i)
```

#### 入力

入力の最初の行に、ヒープのサイズ $H$ が与えられます。続いて、ヒープの節点の値を表す $H$ 個の整数が節点の番号が 1 から $H$ に向かって順番に空白区切りで与えられます。

#### 出力

max-ヒープの節点の値を節点の番号が 1 から $H$ に向かって順番に１行に出力してください。各値の直前に１つの空白文字を出力してください。

#### サンプル

入力

```
10
4 1 3 2 16 9 10 14 8 7
```

出力

```
 16 14 10 8 7 9 3 2 4 1
```

### ソリューション

```C++
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
void maxHeapify(vector<int> &A, int i)
{
	int n = A.size();
	int left = 2 * i + 1;
	int right = left + 1;
	int largest;
	if (left < n && A[left] > A[i])//先和左子比
		largest = left;
	else
		largest = i;
	if (right < n && A[right] > A[largest])//再和右子比
		largest = right;
	if (largest != i)//如果最大的是左子或右子，则将当前节点和子节点互换
	{
		swap(A[i], A[largest]);
		maxHeapify(A, largest);//和下面的子树接着换
	}
}
void CreateMaxHeap(vector<int> &A)
{
	int size = A.size();
	for (int i = size / 2; i >= 0; i--)
		maxHeapify(A, i);
}
int main()
{
	int n;
	cin.tie(0);
	cout.tie(0);
	cin >> n;
	vector<int> Heap(n,0);
	for (int i = 0; i < n; i++)
		cin >> Heap[i];
	CreateMaxHeap(Heap);
	for (int i = 0; i < n; i++)
		cout << ' ' << Heap[i];
	cout << endl;
	return 0;
}
```

## 3.優先順位キュー

### デスクリプション

1. 優先度付きキュー (priority queue) は各要素がキーを持ったデータの集合 $S$ を保持するデータ構造で、主に次の操作を行います：

   - $insert(S,k)$: 集合 $S$ に要素 $k$ を挿入する
   - $extractMax(S)$: 最大のキーを持つ $S$ の要素を $S$ から削除してその値を返す

   優先度付きキュー $S$ に対して $insert(S,k)$、$extractMax(S)$を行うプログラムを作成してください。ここでは、キューの要素を整数とし、それ自身をキーとみなします。

#### 入力

優先度付きキュー $S$への複数の命令が与えられます。各命令は、insert $k$、extractまたはendの形式で命令が１行に与えられます。ここで $k$ は挿入する整数を表します。

end命令が入力の終わりを示します。

#### 出力

extract命令ごとに、優先度付きキュー$S$から取り出される値を１行に出力してください。

#### サンプル

入力

```
insert 8
insert 2
extract
insert 10
extract
insert 11
extract
extract
end
```

出力

```
8
10
11
2
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
class Pro_Queue
{
private:
	vector<int> Array;
public:
	void adjust_down(int k);//上浮
	void adjust_up(int k);//下沉
	void push(int value);
	int pop();
	bool isEmpty();
	int getsize();
};
void Pro_Queue::adjust_down(int k)//向下调整就和左右子比，若父节点小就和儿子换直到最后
{
	int child = k * 2 + 1;
	while (child < getsize())
	{
		if (child + 1 < getsize() && Array[child] < Array[child + 1])
			child++;
		if (Array[k] < Array[child])
		{
			swap(Array[k], Array[child]);
			k = child;
			child = 2 * k + 1;
		}
		else
		{
			break;
		}
	}
}
void Pro_Queue::adjust_up(int k)//向下调整就和父节点比，若子节点大就和父亲换直到最后
{
	int parent = (k - 1) / 2;
	while (k != 0)
	{
		if (Array[k] > Array[parent])
		{
			swap(Array[k], Array[parent]);
			k = parent;
			parent = (k - 1) / 2;
		}
		else
			break;
	}
}
void Pro_Queue::push(int value)
{
	Array.push_back(value);
	adjust_up(getsize() - 1);
}
int Pro_Queue::pop()
{
	if (!isEmpty())
	{
		int max = Array[0];
		swap(Array[0], Array[getsize()-1]);
		Array.pop_back();
		adjust_down(0);
		return max;
	}
}
bool Pro_Queue::isEmpty()
{
	return getsize() == 0;
}
int Pro_Queue::getsize()
{
	return Array.size();
}
int main()
{
	Pro_Queue pq;
	string inst;
	int value;
	while(true)
	{
		cin >> inst;
		if (inst == "insert")
		{
			cin >> value;
			pq.push(value);
		}
		else if (inst == "extract")
		{
			cout << pq.pop() << endl;
		}
		else if (inst == "end")
			break;
	}
	return 0;
}
```

## 4.ヒープソート

### デスクリプション

以下のように、ソートアルゴリズムはそれらの計算量や安定性など、様々な特徴を持ちます。

![image-20230322171507154](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230322171507154.png)

ヒープソート（Heap Sort）はヒープのデータ構造に基づくソートで、入力配列内でソート処理を達成できる（メモリ効率のよい）、高速なソートアルゴリズムです。ヒープソートは、次のように実装することができます

```c++
1  maxHeapify(A, i)
2      l = left(i)
3      r = right(i)
4      // select the node which has the maximum value
5      if l ≤ heapSize and A[l] > A[i]
6          largest = l
7      else 
8          largest = i
9      if r ≤ heapSize and A[r] > A[largest]
10         largest = r
11
12     if largest ≠ i　
13         swap A[i] and A[largest]
14         maxHeapify(A, largest) 
15
16 heapSort(A):
17     // buildMaxHeap
18     for i = N/2 downto 1:
19         maxHeapify(A, i)
20     // sort
21     heapSize ← N
22     while heapSize ≥ 2:
23         swap(A[1], A[heapSize])
24         heapSize--
25         maxHeapify(A, 1)
```

一方、ヒープソートでは、離れた要素が頻繁にスワップされ、連続でない要素へのランダムアクセスが多く発生してしまいます。

$N$要素の数列$A$が与えられます。最大ヒープを満たし、ヒープソートを行ったときに疑似コード25行目のmaxHeapifyにおけるスワップ回数の総数が最大となるような数列$A$の順列を１つ出力してください

#### 入力

1行目に、数列の長さを表す整数 $N$ が与えられます。２行目に、$N$ 個の整数が空白区切りで与えられます。

#### 出力

条件を満たす数列を 1 行に出力してください。数列の連続する要素は１つの空白で区切って出力してください。

この問題では、１つの入力に対して複数の解答があります。条件を満たす出力は全て正解となります。

#### サンプル

入力

```
8
1 2 3 5 9 12 15 23
```

出力

```
23 9 15 2 5 3 12 1
```

### ソリューション

```c++
//这里的题解为纯堆排序，与题目无关
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
void maxHeapify(vector<int>& A, int n, int i)
{
	int left = 2 * i + 1;
	int right = left + 1;
	int largest = i;
	if (left < n && A[i] < A[left])
		largest = left;
	if (right < n && A[largest] < A[right])
		largest = right;
	if (largest != i)
	{
		swap(A[i], A[largest]);
		maxHeapify(A, n, largest);
	}		
}
void Heapsort(vector<int>& A)
{
	for (int i = (A.size() - 1 - 1) / 2; i >= 0; i--)
		maxHeapify(A, A.size(), i);
	for (int i = A.size() - 1; i >= 0; i--)
	{
		swap(A[i], A[0]);//最后一个元素和大顶堆的顶交换
		maxHeapify(A, i, 0);//其他元素维护大顶堆性质，下一次继续
	}
}
int main()
{
	int n;
	cin >> n;
	int val;
	vector<int> Heap;
	for (int i = 0; i < n; i++)
	{
		cin >> val;
		Heap.push_back(val);
	}
	Heapsort(Heap);
	for (int i = 0; i < n; i++)
	{
		if (i == 0)
			cout << Heap[i];
		else
			cout << ' ' << Heap[i];
	}
	cout << endl;
}
```