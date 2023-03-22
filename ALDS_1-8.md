# ALDS_1-8

## 1.二分探索木I

### デスクリプション

探索木は、挿入、検索、削除などの操作が行えるデータ構造で、辞書あるいは優先度付きキューとして用いることができます。探索木の中でも最も基本的なものが二分探索木です。二分探索木は、各節点にキーを持ち、次に示す二分探索木条件(Binary search tree property) を常に満たすように構築されます：

- $x$ を２分探索木に属するある節点とする。 $y$ を $x$ の左部分木に属する節点とすると、$y$ のキー $≤x$のキーである。また、$y$ を $x$ の右部分木に属する節点とすると、$x $のキー $≤y$のキーである。

次の図は二分探索木の例です。

![image-20230315165928678](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230315165928678.png)


例えば、キーが80の節点の左部分木に属する節点のキーは80以下であり、右部分木に属する節点のキーは80以上になっています。二分探索木に中間順巡回を行うと、昇順に並べられたキーの列を得ることができます。

二分探索木は、データの挿入や削除が行われても常にこのような条件が全ての節点で成り立つように実装しなければなりません。リストと同様に、節点をポインタで連結することで木を表し、各節点には値（キー）に加えその親、左の子、右の子へのポインタを持たせます。

二分探索木 $T$ に新たに値 $v$ を挿入するには以下の疑似コードに示す insert を実行します。insert は、キーが $v$、左の子が $NIL$、右の子が $NIL$ であるよな節点 $z$ を受け取り、$T$ の正しい位置に挿入します

```c++
1 insert(T, z)
2     y = NIL // x の親
3     x = 'T の根'
4     while x ≠ NIL
5         y = x // 親を設定
6         if z.key < x.key
7             x = x.left // 左の子へ移動
8         else 
9             x = x.right // 右の子へ移動
10    z.p = y
11
12    if y == NIL // T が空の場合
13        'T の根' = z
14    else if z.key < y.key
15        y.left = z // z を y の左の子にする
16    else 
17        y.right = z // z を y の右の子にする
```

二分探索木 $T$ に対し、以下の命令を実行するプログラムを作成してください。

- insert $k$: $T$にキー $k$ を挿入する。
- print: キーを木の中間順巡回(inorder tree walk)と先行順巡回(preorder tree walk)アルゴリズムで出力する。

挿入のアルゴリズムは上記疑似コードに従ってください

#### 入力

入力の最初の行に、命令の数 $m$ が与えられます。続く $m$ 行目に、insert $k$ または print の形式で命令が１行に与えられます。

#### 出力

print命令ごとに、中間順巡回アルゴリズム、先行順巡回アルゴリズムによって得られるキーの順列をそれぞれ１行に出力してください。各キーの前に１つの空白を出力してください。

#### サンプル

入力

```
8
insert 30
insert 88
insert 12
insert 1
insert 20
insert 17
insert 25
print
```

出力

```
 1 12 17 20 25 30 88
 30 12 1 20 17 25 88
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
struct Node
{
	Node* leftchild;
	Node* rightchild;
	int key;
	Node(int& k)
		:key(k)
		, leftchild(nullptr)
		, rightchild(nullptr)
	{}
};
Node* insert(Node* root, int& key)
{
	if (root == nullptr)
	{
		root = new Node(key);
		return root;
	}
	else if (key < root->key)
	{
		root->leftchild = insert(root->leftchild, key);
		return root;
	}
	else if (key > root->key)
	{
		root->rightchild = insert(root->rightchild, key);
		return root;
	}
	else
		return nullptr;
}
void PreOrder(Node* root)
{
	if (root == nullptr)
		return;
	cout << ' ' << root->key;
	PreOrder(root->leftchild);
	PreOrder(root->rightchild);
}
void InOrder(Node* root)
{
	if (root == nullptr)
		return;
	InOrder(root->leftchild);
	cout << ' ' << root->key;
	InOrder(root->rightchild);
}
void print(Node* root)
{
	InOrder(root);
	cout << endl;
	PreOrder(root);
	cout << endl;
}
int main()
{
	Node* root(0);
	string inst;
	int key;
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> inst;
		if (inst == "insert")
		{
			cin >> key;
			if (i == 0)
			{
				root = new Node(key);
			}
			else
			{
				insert(root, key);
			}
		}
		else if (inst == "print")
			print(root);
	}
}
```

## 2.二分探索木II

### デスクリプション

A: Binary Search Tree I に、find 命令を追加し、二分探索木 $T$ に対し、以下の命令を実行するプログラムを作成してください。

- insert $k$: $T$にキー $k$を挿入する。
- find $k$: $T$にキー $k$ が存在するか否かを報告する。
- print: キーを木の中間順巡回(inorder tree walk)と先行順巡回(preorder tree walk)アルゴリズムで出力する

#### 入力

入力の最初の行に、命令の数 $m$ が与えられます。続く$m$ 行目に、insert $k$、find $k$ またはprintの形式で命令が１行に与えられます。

#### 出力

find $k$ 命令ごとに、$T$ に $k$ が含まれる場合 yes と、含まれない場合 no と１行に出力してください。

さらに print 命令ごとに、中間順巡回アルゴリズム、先行順巡回アルゴリズムによって得られるキーの順列をそれぞれ１行に出力してください。各キーの前に１つの空白を出力してください。

#### サンプル

入力

```
10
insert 30
insert 88
insert 12
insert 1
insert 20
find 12
insert 17
insert 25
find 16
print
```

出力

```
yes
no
 1 12 17 20 25 30 88
 30 12 1 20 17 25 88
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
struct Node
{
	Node* leftchild;
	Node* rightchild;
	int key;
	Node(int& k)
		:key(k)
		, leftchild(nullptr)
		, rightchild(nullptr)
	{}
};
Node* insert(Node* root, int& key)
{
	if (root == nullptr)
	{
		root = new Node(key);
		return root;
	}
	else if (key < root->key)
	{
		root->leftchild = insert(root->leftchild, key);
		return root;
	}
	else if (key > root->key)
	{
		root->rightchild = insert(root->rightchild, key);
		return root;
	}
	else
		return nullptr;
}
void PreOrder(Node* root)
{
	if (root == nullptr)
		return;
	cout << ' ' << root->key;
	PreOrder(root->leftchild);
	PreOrder(root->rightchild);
}
void InOrder(Node* root)
{
	if (root == nullptr)
		return;
	InOrder(root->leftchild);
	cout << ' ' << root->key;
	InOrder(root->rightchild);
}
void print(Node* root)
{
	InOrder(root);
	cout << endl;
	PreOrder(root);
	cout << endl;
}
bool find(Node* root, int& key)
{
	if (root == nullptr)
	{
		cout << "no" << endl;
		return false;
	}
	else
	{
		if (key < root->key)
			find(root->leftchild, key);
		else if (key > root->key)
			find(root->rightchild, key);
		else
		{
			cout << "yes" << endl;
			return true;
		}
	}
	return false;
}
int main()
{
	Node* root(0);
	string inst;
	int key;
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> inst;
		if (inst == "insert")
		{
			cin >> key;
			if (i == 0)
			{
				root = new Node(key);
			}
			else
			{
				insert(root, key);
			}
		}
		else if (inst == "find")
		{
			cin >> key;
			find(root, key);
		}
		else if (inst == "print")
			print(root);
	}
}
```

## 3.２分探索木III

### デスクリプション

B: Binary Search Tree II に、delete 命令を追加し、二分探索木 $T$ に対し、以下の命令を実行するプログラムを作成してください。

- insert $k$: $T$ にキー $k$ を挿入する。
- find $k$: $T$ にキー $k$ が存在するか否かを報告する。
- delete $k$: キー $k$ を持つ節点を削除する。
- print: キーを木の中間順巡回(inorder tree walk)と先行順巡回(preorder tree walk)アルゴリズムで出力する。

二分探索木 $T$ から与えられたキー $k$ を持つ節点 $z$ を削除する delete k は以下の３つの場合を検討したアルゴリズムに従い、二分探索木条件を保ちつつ親子のリンク（ポインタ）を更新します：

1. $z$ が子を持たない場合、$z$ の親 $p$ の子（つまり $z$）を削除する。
2. $z$ がちょうど１つの子を持つ場合、$z$ の親の子を $z$ の子に変更、$z$ の子の親を $z$ の親に変更し、$z$ を木から削除する。
3. $z$ が子を２つ持つ場合、$z$ の次節点 $y$ のキーを $z$ のキーへコピーし、$y$ を削除する。$y$ の削除では 1. または 2. を適用する。ここで、$z$ の次節点とは、中間順巡回で $z$ の次に得られる節点である。

#### 入力

1行目にカードの枚数 $n$ が与えられます。

2行目以降で $n$ 枚のカードが与えられます。各カードは絵柄を表す１つの文字と数（整数）のペアで１行に与えられます。絵柄と数は１つの空白で区切られています

#### 出力

1行目に、この出力が安定か否か（StableまたはNot stable）を出力してください。

2行目以降で、入力と同様の形式で整列されたカードを順番に出力してください（$n$ を出力する必要はありません）。

#### サンプル

入力

```
18
insert 8
insert 2
insert 3
insert 7
insert 22
insert 1
find 1
find 2
find 3
find 4
find 5
find 6
find 7
find 8
print
delete 3
delete 7
print
```

出力

```
yes
yes
yes
no
no
no
yes
yes
 1 2 3 7 8 22
 8 2 1 3 7 22
 1 2 8 22
 8 2 1 22
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
struct Node
{
	Node* leftchild;
	Node* rightchild;
	int key;
	Node(int& k)
		:key(k)
		, leftchild(nullptr)
		, rightchild(nullptr)
	{}
};
Node* insert(Node* root, int& key)
{
	if (root == nullptr)
	{
		root = new Node(key);
		return root;
	}
	else if (key < root->key)
	{
		root->leftchild = insert(root->leftchild, key);
		return root;
	}
	else if (key > root->key)
	{
		root->rightchild = insert(root->rightchild, key);
		return root;
	}
	else
		return nullptr;
}
void PreOrder(Node* root)
{
	if (root == nullptr)
		return;
	cout << ' ' << root->key;
	PreOrder(root->leftchild);
	PreOrder(root->rightchild);
}
void InOrder(Node* root)
{
	if (root == nullptr)
		return;
	InOrder(root->leftchild);
	cout << ' ' << root->key;
	InOrder(root->rightchild);
}
void print(Node* root)
{
	InOrder(root);
	cout << endl;
	PreOrder(root);
	cout << endl;
}
bool find(Node* root, int& key)
{
	if (root == nullptr)
	{
		cout << "no" << endl;
		return false;
	}
	else
	{
		if (key < root->key)
			find(root->leftchild, key);
		else if (key > root->key)
			find(root->rightchild, key);
		else
		{
			cout << "yes" << endl;
			return true;
		}
	}
	return false;
}
Node* remove(Node* root, int& key)
{
	if (root == nullptr)
		return nullptr;
	if (key < root->key)//根据大小分类讨论
	{
		root->leftchild=remove(root->leftchild, key);
	}
	else if (key > root->key)
	{
		root->rightchild=remove(root->rightchild, key);
	}
	else
	{
		if (root->leftchild == nullptr && root->rightchild == nullptr)
		{
			root = nullptr;
		}
		else if (root->rightchild&&root->leftchild == nullptr)
		{
			root = root->rightchild;
		}
		else if (root->leftchild&&root->rightchild == nullptr)
		{
			root = root->leftchild;
		}
		else
		{
			Node* RightFirst = root->rightchild;
			while (RightFirst->leftchild)
				RightFirst = RightFirst->leftchild;
			root->key = RightFirst->key;
			root->rightchild=remove(root->rightchild, root->key);
		}
	}
	return root;
}
int main()
{
	Node* root(0);
	string inst;
	int key;
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> inst;
		if (inst == "insert")
		{
			cin >> key;
			if (i == 0)
			{
				root = new Node(key);
			}
			else
			{
				insert(root, key);
			}
		}
		else if (inst == "find")
		{
			cin >> key;
			find(root, key);
		}
		else if (inst == "print")
			print(root);
		else if (inst == "delete")
		{
			cin >> key;
			remove(root, key);
		}
	}
}
```

## 4.Treap

### デスクリプション

二分探索木は、挿入されるデータ列の特徴によっては、偏った木になり、検索・挿入・削除操作の効率が悪くなります。例えば、整列された$n$個のデータが順番に挿入されれば、木はリストのような形になり、その高さは$n$になります。挿入されるデータが固定されていれば、要素をランダムにシャッフルすることにより平衡な木を構築することができるでしょう。しかし、データ構造としての二分木では要求に応じて異なる操作が繰り返し行われるため、適宜データを一つずつ処理しながら、常に平衡な状態を保つ必要があります。

二分木の各節点に、ランダムに選択された優先度を割り当て、以下の条件を満たすように節点を順序付けることによって、平衡な二分木を保つことができます。ここで、各節点のキー(key)と優先度(priority)はそれぞれ重複がないものとします。

- **二分探索木条件.** $v$が$u$の**左の子**なら$v.key<u.key$ かつ $v$が$u$の**右の子**なら $u.key<v.key$
- **ヒープ条件.**$v$が$u$の**子**なら$v.priority<u.priority$

このような木を、二分探索木とヒープの特徴からTreap ( tree + heap ) と呼びます。

例えば、次の図はTreapの例です。

![image-20230316122819109](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230316122819109.png)

**挿入**
Treapに新たにデータを挿入するには、キーに加えランダムに選択した優先度を割り当てた節点を、まずは通常の二分探索木と同様の方法で挿入します。例えば、上のTreapにkey = 6, priority = 90 である節点を挿入すると次のようになります

![image-20230316122837971](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230316122837971.png)

このままの状態では、ヒープ条件を破ってしまうため、ヒープ条件を満たすまで**回転**を繰り返します。回転とは、次の図のように、二分探索木条件を満たしつつ、親子関係を逆転させる操作です。

![image-20230316122856782](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230316122856782.png)

回転は次のプログラムのようにポインタを繋ぎ変えます。

```c++
rightRotate(Node t)
    Node s = t.left
    t.left = s.right
    s.right = t
    return s // root of the subtree
```

```c++
leftRotate(Node t)
    Node s = t.right
    t.right = s.left
    s.left = t
    return s // root of the subtree
```

上のTreapに回転操作を行うと以下のように二分探索木が構築されます。

![image-20230316122937555](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230316122937555.png)

条件を満たすようにTreapに新しい要素を挿入するinsert操作は以下のようになります。

```c++
insert(Node t, int key, int priority)            // 再帰的に探索
    if t == NIL
        return Node(key, priority)               // 葉に到達したら新しい節点を生成して返す
    if key == t.key
        return t                                 // 重複したkeyは無視

    if key < t.key                               // 左の子へ移動
        t.left = insert(t.left, key, priority)   // 左の子へのポインタを更新
        if t.priority < t.left.priority          // 左の子の方が優先度が高い場合右回転
            t = rightRotate(t)
    else                                         // 右の子へ移動
        t.right = insert(t.right, key, priority) // 右の子へのポインタを更新
        if t.priority < t.right.priority         // 右の子の方が優先度が高い場合左回転
            t = leftRotate(t)

  return t
```

**削除**
Treapの節点を削除する場合は、以下の手順で対象となる節点を回転によって葉まで移動した後、削除します。

```c++
delete(Node t, int key)
    if t == NIL
        return NIL
    if key < t.key                                // 削除対象を検索
        t.left = delete(t.left, key)
    else if key > t.key
        t.right = delete(t.right, key)
    else
        return _delete(t, key)
    return t

_delete(Node t, key) // 削除対象の節点の場合
    if t.left == NIL && t.right == NIL           // 葉の場合
        return NIL
    else if t.left == NIL                        // 右の子のみを持つ場合左回転
        t = leftRotate(t)
    else if t.right == NIL                       // 左の子のみを持つ場合右回転
        t = rightRotate(t)
    else                                         // 左の子と右の子を両方持つ場合
        if t.left.priority > t.right.priority    // 優先度が高い方を持ち上げる
            t = rightRotate(t)
        else
            t = leftRotate(t)
    return delete(t, key)
```

Treap $T$ に対して、以下の命令を、上記のアルゴリズムに基づいて実行するプログラムを作成してください。

- insert ($k$, $p$): $T$ にキーが $k$、優先度が$p$の要素 を挿入する。
- find ($k$): $T$ にキー $k$ が存在するか否かを報告する。
- delete ($k$): キー $k$ を持つ節点を削除する。
- print(): キーを木の中間順巡回(inorder tree walk)と先行順巡回(preorder tree walk)アルゴリズムで出力する。

#### 入力

入力の最初の行に、命令の数 $m$ が与えられます。続く$m$ 行に、insert $k$  $p$、find $k$、delete $k$ または print の形式で命令が１行に与えられます

#### 出力

find $k$ 命令ごとに、$T$ に $k$ が含まれる場合 yes と、含まれない場合 no と１行に出力してください。

さらに print 命令ごとに、中間順巡回アルゴリズム、先行順巡回アルゴリズムによって得られるキーの順列をそれぞれ１行に出力してください。各キーの前に１つの空白を出力してください。

#### サンプル

入力

```
16
insert 35 99
insert 3 80
insert 1 53
insert 14 25
insert 80 76
insert 42 3
insert 86 47
insert 21 12
insert 7 10
insert 6 90
print
find 21
find 22
delete 35
delete 99
print
```

出力

```
 1 3 6 7 14 21 35 42 80 86
 35 6 3 1 14 7 21 80 42 86
yes
no
 1 3 6 7 14 21 42 80 86
 6 3 1 80 14 7 21 42 86
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
struct Node
{
	Node* leftchild;
	Node* rightchild;
	int key;
	int priority;
	Node(){}
	Node(int& k, int& p)
		:key(k)
		, priority(p)
		, leftchild(nullptr)
		, rightchild(nullptr)
	{}
};
Node* remove(Node* root, int& key);
Node* _remove(Node* root, int& key);
Node* leftRotate(Node* root)
{
	Node* temp = root->rightchild;
	root->rightchild = temp->leftchild;
	temp->leftchild = root;
	return temp;
}
Node* rightRotate(Node* root)
{
	Node* temp = root->leftchild;
	root->leftchild = temp->rightchild;
	temp->rightchild = root;
	return temp;
}
Node* insert(Node* root, int& key, int& priority)
{
	if (root == nullptr)
	{
		root = new Node(key, priority);
		return root;
	}
	if (key == root->key)
		return root;
	else if (key < root->key)
	{
		root->leftchild = insert(root->leftchild, key, priority);
		if (root->priority < root->leftchild->priority)
			root = rightRotate(root);
	}
	else if (key > root->key)
	{
		root->rightchild = insert(root->rightchild, key, priority);
		if (root->priority < root->rightchild->priority)
			root = leftRotate(root);
	}
	return root;
}
void PreOrder(Node* root)
{
	if (root == nullptr)
		return;
	cout << ' ' << root->key;
	PreOrder(root->leftchild);
	PreOrder(root->rightchild);
}
void InOrder(Node* root)
{
	if (root == nullptr)
		return;
	InOrder(root->leftchild);
	cout << ' ' << root->key;
	InOrder(root->rightchild);
}
void print(Node* root)
{
	InOrder(root);
	cout << endl;
	PreOrder(root);
	cout << endl;
}
bool find(Node* root, int& key)
{
	if (root == nullptr)
	{
		cout << "no" << endl;
		return false;
	}
	else
	{
		if (key < root->key)
			find(root->leftchild, key);
		else if (key > root->key)
			find(root->rightchild, key);
		else
		{
			cout << "yes" << endl;
			return true;
		}
	}
	return false;
}
Node* remove(Node* root, int& key)
{
	if (root == nullptr)
		return nullptr;
	if (key < root->key)
	{
		root->leftchild = remove(root->leftchild, key);
	}
	else if (key > root->key)
	{
		root->rightchild = remove(root->rightchild, key);
	}
	else
		return _remove(root, key);
	return root;
}
Node* _remove(Node* root, int& key)
{
	if (root->leftchild == nullptr && root->rightchild == nullptr)
		return nullptr;
	else if (root->leftchild == nullptr)
		root = leftRotate(root);
	else if (root->rightchild == nullptr)
		root = rightRotate(root);
	else
	{
		if (root->leftchild->priority > root->rightchild->priority)
			root = rightRotate(root);
		else
			root = leftRotate(root);
	}
	return remove(root, key);
}
int main()
{
	Node* root(0);
	string inst;
	int key,priority;
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> inst;
		if (inst == "insert")
		{
			cin >> key >> priority;
			if (i == 0)
			{
				root = new Node(key, priority);
			}
			else
			{
				insert(root, key, priority);
			}
		}
		else if (inst == "find")
		{
			cin >> key;
			find(root, key);
		}
		else if (inst == "print")
			print(root);
		else if (inst == "delete")
		{
			cin >> key;
			root=remove(root, key);
		}
	}
}
```