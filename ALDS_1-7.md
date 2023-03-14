# ALDS_1-7

## 1.根付き木

### デスクリプション

与えられた根付き木 $T$ の各節点 $u$ について、以下の情報を出力するプログラムを作成してください。

- $u$ の節点番号
- $u$ の親の節点番号
- $u$ の深さ
- $u$ の節点の種類（根、内部節点または葉）
- $u$ の子のリスト

ここでは、与えられる木は $n$個の節点を持ち、それぞれ $0$ から $n−1$ の番号が割り当てられているものとします。

#### 入力

入力の最初の行に、節点の個数 $n$ が与えられます。続く $n$ 行目に、各節点の情報が次の形式で１行に与えられます。

$id\ k\ c_1\ c_2\ ... c_k$

$id$ は節点の番号、$k$ は次数を表します。$c_1\ c_2\ ...\ c_k$ は 1 番目の子の節点番号、... $k$ 番目の子の節点番号を示します

#### 出力

次の形式で節点の情報を出力してください。節点の情報はその番号が小さい順に出力してください。

node $id$: parent = $p$, depth = $d$, $type$ $[c_1...c_k]$

$p$は親の番号を示します。ただし、親を持たない場合は -1 とします。$d$は節点の深さを示します。

$type$は根、内部節点、葉をそれぞれあらわす root、internal node、leaf の文字列のいずれかです。ただし、根が葉や内部節点の条件に該当する場合は root とします。

$c_1...c_k$ は子のリストです。順序木とみなし入力された順に出力してください。カンマ空白区切りに注意してください。出力例にて出力形式を確認してください。

#### サンプル

入力

```
13
0 3 1 4 10
1 2 2 3
2 0
3 0
4 3 5 6 7
5 0
6 0
7 2 8 9
8 0
9 0
10 2 11 12
11 0
12 0
```

出力

```
node 0: parent = -1, depth = 0, root, [1, 4, 10]
node 1: parent = 0, depth = 1, internal node, [2, 3]
node 2: parent = 1, depth = 2, leaf, []
node 3: parent = 1, depth = 2, leaf, []
node 4: parent = 0, depth = 1, internal node, [5, 6, 7]
node 5: parent = 4, depth = 2, leaf, []
node 6: parent = 4, depth = 2, leaf, []
node 7: parent = 4, depth = 2, internal node, [8, 9]
node 8: parent = 7, depth = 3, leaf, []
node 9: parent = 7, depth = 3, leaf, []
node 10: parent = 0, depth = 1, internal node, [11, 12]
node 11: parent = 10, depth = 2, leaf, []
node 12: parent = 10, depth = 2, leaf, []xxxxxxxxxx 
node 0: parent = -1, depth = 0, root, [1, 4, 10]
node 1: parent = 0, depth = 1, internal node, [2, 3]
node 2: parent = 1, depth = 2, leaf, []n
ode 3: parent = 1, depth = 2, leaf, []
node 4: parent = 0, depth = 1, internal node, [5, 6, 7]
node 5: parent = 4, depth = 2, leaf, []
node 6: parent = 4, depth = 2, leaf, []
node 7: parent = 4, depth = 2, internal node, [8, 9]
node 8: parent = 7, depth = 3, leaf, []
node 9: parent = 7, depth = 3, leaf, []
node 10: parent = 0, depth = 1, internal node, [11, 12]
node 11: parent = 10, depth = 2, leaf, []
node 12: parent = 10, depth = 2, leaf, []0 1 2 2 3 3 5
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
#define MAX 100005
#define NIL -1
struct Node
{
	Node()
	{
		parent = -1;
		depth = subnum = 0;
	}
	int parent;
	int subnum;
	int depth;
	vector<int> sub;
};
void Depth(Node* Tree, int root)
{
	for (int i = 0; i < Tree[root].subnum; i++)
	{
		Tree[Tree[root].sub[i]].depth = Tree[root].depth + 1;//逐层往下找
		Depth(Tree, Tree[root].sub[i]);
	}
}
int main()
{
	int n,sub_id,id,subnum,root_id;
	cin >> n;
	Node* Tree = new Node[n];
	for (int i = 0; i < n; i++)
	{
		cin >> id >> subnum;
		for (int j = 0; j < subnum; j++)
		{
			cin >> sub_id;
			Tree[id].sub.push_back(sub_id);
			Tree[id].subnum++;
			Tree[sub_id].parent = id;
		}
	}
	for (root_id = 0; root_id < n; root_id++)
	{
		if (Tree[root_id].parent == -1)
			break;
	}
	Depth(Tree, root_id);
	for (int i = 0; i < n; i++) 
	{
		if (Tree[i].parent == -1) 
		{
			printf("node %d: parent = -1, depth = 0, root, ", i);
		}
		else if (Tree[i].subnum == 0) 
		{
			printf("node %d: parent = %d, depth = %d, leaf, ", i, Tree[i].parent, Tree[i].depth);
		}
		else 
		{
			printf("node %d: parent = %d, depth = %d, internal node, ", i, Tree[i].parent, Tree[i].depth);
		}
		printf("[");
		for (int j = 0; j < Tree[i].subnum - 1; j++) 
		{
			printf("%d, ", Tree[i].sub[j]);
		}
		if (Tree[i].subnum > 0) 
		{
			printf("%d", Tree[i].sub[Tree[i].subnum - 1]);
		}
		printf("]\n");
	}
}
```

## 2.Partition

### デスクリプション

与えられた二分木 $T$ の各節点 $u$ について、以下の情報を出力するプログラムを作成してください。

- $u$ の節点番号
- $u$ の親
- $u$ の兄弟
- $u$ の子の数
- $u$の深さ
- $u$ の高さ
- 節点の種類（根、内部節点または葉）

ここでは、与えられる二分木は $n$ 個の節点を持ち、それぞれ 00 から $n−1$ の番号が割り当てられているものとします

#### 入力

入力の最初の行に、節点の個数 $n$ が与えられます。続く $n$ 行目に、各節点の情報が以下の形式で１行に与えられます

#### 出力

次の形式で節点の情報を出力してください。節点の情報はその番号が小さい順に出力してください。

node $id$: parent = $p$, depth = $d$, $type$ $[c_1...c_k]$

$p$は親の番号を示します。ただし、親を持たない場合は -1 とします。$d$は節点の深さを示します。

$type$は根、内部節点、葉をそれぞれあらわす root、internal node、leaf の文字列のいずれかです。ただし、根が葉や内部節点の条件に該当する場合は root とします。

$c_1...c_k$ は子のリストです。順序木とみなし入力された順に出力してください。カンマ空白区切りに注意してください。出力例にて出力形式を確認してください。

#### サンプル

入力

```
9
0 1 4
1 2 3
2 -1 -1
3 -1 -1
4 5 8
5 6 7
6 -1 -1
7 -1 -1
8 -1 -1
```

出力

```
node 0: parent = -1, sibling = -1, degree = 2, depth = 0, height = 3, root
node 1: parent = 0, sibling = 4, degree = 2, depth = 1, height = 1, internal node
node 2: parent = 1, sibling = 3, degree = 0, depth = 2, height = 0, leaf
node 3: parent = 1, sibling = 2, degree = 0, depth = 2, height = 0, leaf
node 4: parent = 0, sibling = 1, degree = 2, depth = 1, height = 2, internal node
node 5: parent = 4, sibling = 8, degree = 2, depth = 2, height = 1, internal node
node 6: parent = 5, sibling = 7, degree = 0, depth = 3, height = 0, leaf
node 7: parent = 5, sibling = 6, degree = 0, depth = 3, height = 0, leaf
node 8: parent = 4, sibling = 5, degree = 0, depth = 2, height = 0, leaf
```

### ソリューション

```C++
#include <stdio.h>
#include <vector>

using namespace std;

struct Node{
	Node(){
		brother = parent = -1;
		height = depth = num_of_children = 0;
	}
	int parent,brother,depth,num_of_children,height;
	vector<int> children;
};

void calcDepth(Node nodes[],int root_id){
	for(int i = 0;i < nodes[root_id].num_of_children;i++){
		nodes[nodes[root_id].children[i]].depth = nodes[root_id].depth+1;//左右找，和链式的left/right同理
		calcDepth(nodes,nodes[root_id].children[i]);//递归
	}
}

void calcHeight(Node nodes[],int height,int parent){
	nodes[parent].height = max(nodes[parent].height,height+1);//左右找，和链式的left/right同理
	if(nodes[parent].parent != -1) calcHeight(nodes,height+1,nodes[parent].parent);//递归
}

int main(){
	int n,id,left_id,right_id,root_id;
	scanf("%d",&n);
	Node nodes[n];

	for(int i = 0; i < n; i++){
		scanf("%d %d %d",&id,&left_id,&right_id);
		if(left_id != -1){
			nodes[id].children.push_back(left_id);
			nodes[id].num_of_children++;
			nodes[left_id].parent = id;
		}
		if(right_id != -1){
			nodes[id].children.push_back(right_id);
			nodes[id].num_of_children++;
			nodes[right_id].parent = id;
		}
		if(left_id != -1 && right_id != -1){
			nodes[left_id].brother = right_id;
			nodes[right_id].brother = left_id;
		}
	}

	for(root_id = 0; root_id < n; root_id++){
		if(nodes[root_id].parent == -1) break;
	}

	if(n > 1){
		calcDepth(nodes,root_id);

		for(int i = 0; i < n; i++){
			if(nodes[i].num_of_children == 0) calcHeight(nodes,0,nodes[i].parent);
		}
	}

	for(int i = 0; i < n; i++){
		printf("node %d: parent = %d, sibling = %d, degree = %d, depth = %d, height = %d,"
				,i,nodes[i].parent,nodes[i].brother,nodes[i].num_of_children,nodes[i].depth,nodes[i].height);
		if(nodes[i].parent == -1){//判断节点类型
			printf(" root\n");
		}else if(nodes[i].num_of_children == 0){
			printf(" leaf\n");
		}else{
			printf(" internal node\n");
		}

	}
}
```

## 3.クイックソート

### デスクリプション

以下に示すアルゴリズムで、与えられた２分木のすべての節点を体系的に訪問するプログラムを作成してください。

1. 根節点、左部分木、右部分木の順で節点の番号を出力する。これを木の先行順巡回 (preorder tree walk) と呼びます。
2. 左部分木、根節点、右部分木の順で節点の番号を出力する。これを木の中間順巡回 (inorder tree walk) と呼びます。
3. 左部分木、右部分木、根節点の順で節点の番号を出力する。これを木の後行順巡回 (postorder tree walk) と呼びます。

与えられる２分木は $n$ 個の節点を持ち、それぞれ $0$ から $n−1$ の番号が割り当てられているものとします。

#### 入力

入力の最初の行に、節点の個数 $n$ が与えられます。続く $n$ 行目に、各節点の情報が以下の形式で１行に与えられます。

$id\ left\ right$

$id$ は節点の番号、$left$ は左の子の番号、$right$ は右の子の番号を表します。子を持たない場合は $left$ ($right$) は -1 で与えられます。

#### 出力

１行目に"Preorder"と出力し、２行目に先行順巡回を行った節点番号を順番に出力してください。

３行目に"Inorder"と出力し、４行目に中間順巡回を行った節点番号を順番に出力してください。

５行目に"Postorder"と出力し、６行目に後行順巡回を行った節点番号を順番に出力してください。

節点番号の前に１つの空白文字を出力してください。

#### サンプル

入力

```
9
0 1 4
1 2 3
2 -1 -1
3 -1 -1
4 5 8
5 6 7
6 -1 -1
7 -1 -1
8 -1 -1
```

出力

```
Preorder
 0 1 2 3 4 5 6 7 8
Inorder
 2 1 3 0 6 5 7 4 8
Postorder
 2 3 1 6 7 5 8 4 0
```

### ソリューション

```c++
//本题在“解答"这一部分直接自己写前序中序后序的代码作为练习，不为题解

**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void sub(TreeNode* root, vector<int>& res)//前序
    {
        if(root==nullptr)
            return;
        else
        {
            res.push_back(root->val);
            sub(root->left, res);
            sub(root->right, res);
        }
    }
    void sub(TreeNode *root, vector<int>& res)//中序
    {
        if(root==nullptr)
            return;
        else
        {
            sub(root->left,res);
            res.push_back(root->val);
            sub(root->right,res);
        }
    }
    void sub(TreeNode *root, vector<int>& res){//后序
        if(root==nullptr)
            return;
        else
        {
            sub(root->left,res);
            sub(root->right,res);
            res.push_back(root->val);
        }
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        sub(root, result);
        return result;
    }
}；
```

すべての操作の時間複雑度はO(1)である

## 4.Reconstruction of a Tree

### デスクリプション

ある二分木に対して、それぞれ先行順巡回 (preorder tree walk) と中間順巡回 (inorder tree walk) を行って得られる節点の列が与えられるので、その二分木の後行順巡回 (postorder tree walk) で得られる節点の列を出力するプログラムを作成してください。

#### 入力

１行目に二分木の節点の数 $n$が与えられます。
２行目に先行順巡回で得られる節点の番号の列が空白区切りで与えられます。
３行目に中間順巡回で得られる節点の番号の列が空白区切りで与えられます。

節点には $1$ から $n$ までの整数が割り当てられています。$1$が根とは限らないことに注意してください。

最小値を１行に出力してください。

#### サンプル

入力

```
5
1 2 3 4 5
3 2 4 1 5
```

出力

```
3 4 2 5 1
```

### ソリューション

```c++
#include <stdio.h>
#include <queue>

using namespace std;

int n,inputed_order[41] = {0};
queue<int> Queue;

struct Node{
	Node(){
		left_child = right_child = -1;
	};
	int left_child,right_child;
};

void postORDER(Node order[],int id){
	if(order[id].left_child != -1) postORDER(order,order[id].left_child );
	if(order[id].right_child != -1) postORDER(order,order[id].right_child );
	Queue.push(id+1);
}

int findRoot(int inorder[],int left,int right){
	int root = inorder[left];
	for(int i = left+1; i <= right; i++){
		if(inputed_order[root] > inputed_order[inorder[i]]){
			root = inorder[i];
		}
	}
	return root;
}

void reconstruct(int inorder[],Node order[],int root,int left,int right){
	int root_index;
	for(root_index = left; inorder[root_index] != root; root_index++);
	if(left < root_index){
		int new_root = findRoot(inorder,left,root_index-1);
		order[root-1].left_child = new_root-1;
		if(left < root_index-1)	reconstruct(inorder,order,new_root,left,root_index-1);
	}
	if(root_index < right){
		int new_root = findRoot(inorder,root_index+1,right);
		order[root-1].right_child = new_root-1;
		if(root_index+1 < right) reconstruct(inorder,order,new_root,root_index+1,right);
	}
}

int main(){
	scanf("%d",&n);
	int preorder[n],inorder[n];
	Node order[n];

	for(int i = 0; i < n; i++){
		scanf("%d",&preorder[i]);
		inputed_order[preorder[i]] = i;
	}
	for(int i = 0; i < n; i++){
		scanf("%d",&inorder[i]);
	}

	if(n == 1){
		printf("%d\n",preorder[0]);
	}else{
		reconstruct(inorder,order,preorder[0],0,n-1);
		postORDER(order,preorder[0]-1);
		while(!Queue.empty()){
			if(Queue.size() != 1){
				printf("%d ",Queue.front());
				Queue.pop();
			}else{
				printf("%d\n",Queue.front());
				Queue.pop();
			}
		}
	}

}
```