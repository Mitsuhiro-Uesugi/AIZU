# ALDS_1-3

## 1.スタック

### デスクリプション

逆ポーランド記法は、演算子をオペランドの後に記述する数式やプログラムを記述する記法です。例えば、一般的な中間記法で記述された数式 (1+2)*(5+4) は、逆ポーランド記法では 1 2 + 5 4 + * と記述されます。逆ポーランド記法では、中間記法で必要とした括弧が不要である、というメリットがあります。

#### 入力

１つの数式が１行に与えられます。連続するシンボル（オペランドあるいは演算子）は１つの空白で区切られて与えられます。

#### 出力

計算結果を１行に出力してください。

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
void split(string str,char split,vector<string>& des)
{
    istringstream iss(str);	// 输入流
    string token;			// 接收缓冲区
    while (getline(iss, token, split))	// 以split为分隔符
    {
        des.push_back(token);
    }
}
int main()
{
    stack<long long> s;
    long long temp1,temp2;
    vector<string> Gyapo;
    string input;
    getline(cin,input);
    split(input,' ',Gyapo);
    for(int i=0;i<Gyapo.size();i++)
    {
        if(Gyapo[i]=="+"||Gyapo[i]=="-"||Gyapo[i]=="*"||Gyapo[i]=="/")
        /*若为加减乘除则分类讨论并取出两个操作数，由于栈后进先出的特性，所以temp2为第一个操作数*/
        {
            temp1=s.top();
            s.pop();
            temp2=s.top();
            s.pop();
            if(Gyapo[i]=="+")
                s.push(temp2+temp1);
            else if(Gyapo[i]=="-")
                s.push(temp2-temp1);
            else if(Gyapo[i]=="*")
                s.push(temp2*temp1);
            else if(Gyapo[i]=="/")
                s.push(temp2/temp1);
        }
        else
        {
            reverse(Gyapo[i].begin(), Gyapo[i].end());
            int temp = 0;
            for (int j = 0; j < Gyapo[i].size(); j++)
            {
                temp += (Gyapo[i][j] - '0') * pow(10, j);
            }
            s.push(temp);
        }
    }
    cout<<s.top()<<endl;
    s.pop();
    return 0;
}
```

## 2.キュー

### デスクリプション

名前 $name_i$ と必要な処理時間 $time_i$ を持つ *n* 個のプロセスが順番に一列に並んでいます。ラウンドロビンスケジューリングと呼ばれる処理方法では、CPU がプロセスを順番に処理します。各プロセスは最大 *q* ms（これをクオンタムと呼びます）だけ処理が実行されます。*q* ms だけ処理を行っても、まだそのプロセスが完了しなければ、そのプロセスは列の最後尾に移動し、CPU は次のプロセスに割り当てられます。

例えば、クオンタムを 100 msとし、次のようなプロセスキューを考えます。

```c++
A(150) - B(80) - C(200) - D(200)
```

まずプロセス A が 100 ms だけ処理され残りの必要時間 50 ms を保持しキューの末尾に移動します。

```c++
B(80) - C(200) - D(200) - A(50)
```

次にプロセス B が 80 ms だけ処理され、時刻 180 ms で終了し、キューから削除されます

```c++
C(200) - D(200) - A(50)
```

次にプロセス C が 100 ms だけ処理され、残りの必要時間 100 ms を保持し列の末尾に移動します。

```c++
D(200) - A(50) - C(100)
```

このように、全てのプロセスが終了するまで処理を繰り返します。

ラウンドロビンスケジューリングをシミュレートするプログラムを作成してください。

#### 入力

入力の形式は以下の通りです。

```latex
$n$ $q$
$name_1$ $time_1$
$name_2$ $time_2$
...
$name_n$ $time_n$
```

最初の行に、プロセス数を表す整数 *n* とクオンタムを表す整数 *q* が１つの空白区切りで与えられます。

続く *n* 行で、各プロセスの情報が順番に与えられます。文字列 $name_i$ と整数 $time_i$ は１つの空白で区切られています。

#### 出力

プロセスが完了した順に、各プロセスの名前と終了時刻を空白で区切って１行に出力してください

#### サンプル

入力

```c++
5 100
p1 150
p2 80
p3 200
p4 350
p5 20
```

出力

```c++
p2 180
p5 400
p1 450
p3 550
p4 800
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
    pair<string,int> process;
    pair<string,int> temp;
    queue<pair<string,int> > q;
    int n,t,sum=0;
    cin>>n>>t;
    string s;
    int time;
    for(int i=0;i<n;i++)
    {
        cin>>s>>time;
        process.first=s;
        process.second=time;
        q.push(process);
    }
    while(!q.empty())//大于周期减掉再把剩余的时间入队，小于直接处理完加上即可
    {
        if(q.front().second>t)
        {
            sum+=t;
            q.front().second-=t;
            temp=q.front();
            q.pop();
            q.push(temp);
        }
        else
        {
            sum+=q.front().second;
            cout<<q.front().first<<' '<<sum<<endl;
            q.pop();
        }
    }
}
```

## 3.双方向連結リスト

### デスクリプション

以下の命令を受けつける双方向連結リストを実装してください。

- insert x: 連結リストの先頭にキー *x* を持つ要素を継ぎ足す。
- delete x: キー *x* を持つ最初の要素を連結リストから削除する。そのような要素が存在しない場合は何もしない。
- deleteFirst: リストの先頭の要素を削除する。
- deleteLast: リストの末尾の要素を削除する。

#### 入力

入力は以下の形式で与えられます。

```c++
n
command1
command2
...
commandn
```

最初の行に命令数 *n* が与えられます。続く *n* 行に命令が与えられる。命令は上記4つの命令のいずれかです。キーは整数とします。

#### 出力

全ての命令が終了した後の、連結リスト内のキーを順番に出力してください。連続するキーは１つの空白文字で区切って出力してください。

#### サンプル

入力

```c++
7
insert 5
insert 2
insert 3
insert 1
delete 3
insert 6
delete 5
```

出力

```c++
6 1 2
```

### ソリューション

```c++
#include <iostream>
#include <vector>
#include <string>
#include <stack>
#include <thread>
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
struct Node
{
    int val;
    Node* prev;
    Node* next;
    Node() : val(0), next(NULL), prev(NULL) {}
    Node(int x) : val(x), next(NULL), prev(NULL) {}
};
class LinkedList
{
private:
    Node* dummyhead;
    int size;
public:
    LinkedList()
    {
        size = 0;
        dummyhead = new Node(0);
    }
    void Insert(int x);
    void Delete(int x);
    void deleteFirst();
    void deleteLast();
    void print();
};

void LinkedList::Insert(int x)
{
    Node* temp = new Node(x);
    temp->next = dummyhead->next;
    dummyhead->next = temp;
    temp->prev = dummyhead;
    if (temp->next != NULL)
    {
        temp->next->prev = temp;
    }
    size++;
}
void LinkedList::Delete(int x)
{
    if (size <= 0)
        return;
    Node* current = dummyhead;
    while (current != NULL && current->next->val != x)
        current = current->next;
    if (current == NULL)
        return;
    Node* temp = current->next;
    current->next = current->next->next;
    if (current->next != NULL)
    {
        current->next->prev = current;
    }
    delete temp;
    size--;
}
void LinkedList::deleteFirst()
{
    if (size <= 0)
        return;
    else
    {
        Node* temp = dummyhead->next;
        dummyhead->next = dummyhead->next->next;
        if (dummyhead->next != NULL)
        {
            dummyhead->next->prev = dummyhead;
        }
        delete temp;
        size--;
    }
}
void LinkedList::deleteLast()//删最后一个时不要忘记指定倒数第二个的next为nullptr，否则会出现野指针
{
    if (size <= 0)
        return;
    else
    {
        Node* current = dummyhead;
        while (current->next != NULL)
            current = current->next;
        current = current->prev;
        Node* temp = current->next;
        current->next = NULL;
        delete temp;
        size--;
    }
}
void LinkedList::print()
{
    if (size <= 0)
        return;
    Node* current = dummyhead->next;
    while (current&&current->next!=NULL)
    {
        cout << current->val << ' ';
        current = current->next;
    }
    cout << current->val << endl;
}
int main()
{
    int n;
    cin >> n;
    string input;
    LinkedList* List = new LinkedList();
    int num;
    for (int i = 0; i < n; i++)
    {
        cin >> input;
        if (input == "insert" || input == "delete")
        {
            cin >> num;
            if (input == "insert")
            {
                List->Insert(num);
            }
            else
            {
                List->Delete(num);
            }
        }
        else if (input == "deleteFirst")
        {
            List->deleteFirst();
        }
        else if (input == "deleteLast")
        {
            List->deleteLast();
        }
    }
    List->print();
}
```

## 4.スタック

### デスクリプション

地域の治水対策として、洪水の被害状況をシミュレーションで仮想してみよう。

図のように $1×1(m^2)$ の区画からなる格子上に表された地域の模式断面図が与えられるので、地域にできる各水たまりの面積を報告してください。

![image-20230302175756283](C:\Users\Mitsuhiro\AppData\Roaming\Typora\typora-user-images\image-20230302175756283.png)

与えられた地域に対して限りなく雨が降り、地域から溢れ出た水は左右の海に流れ出ると仮定します。 例えば、図の断面図では、左から面積が 4、2、1、19、9 の水たまりができます。

#### 入力

模式断面図における斜面を '/' と '\\'、平地を '\_' で表した文字列が１行に与えられます。例えば、図の模式断面図は文字列 \\///\_/\/\\\\/_/\\///__\\\_\\/_\/_/\ で与えられます。

#### 出力

次の形式で水たまりの面積を出力してください。

#### サンプル

入力

```c++
\\//
```

出力

```c++
4
1 4
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
    stack<pair<int, int> > s1;
    stack<int> s2;
    int p_sum;
    int sum = 0;
    vector<int> v;
    string input;
    int pos;
    getline(cin, input);
    pair<int, int> p;
    for (int i = 0; i < input.size(); i++)
    {
        if (input[i] == '\\')//为'\'则压栈
        {
            s2.push(i);
        }
        else if (!s2.empty() && input[i] == '/')
        {
            pos = s2.top();
            s2.pop();
            p_sum = i - pos;//算出这一行的面积
            while (!s1.empty() && s1.top().first > pos)
            {
                p_sum += s1.top().second;//算出整个池子的面积
                s1.pop();
            }
            p.first = pos;//池子的起始位置
            p.second = p_sum;
            s1.push(p);
        }
    }
    while (!s1.empty())
    {
        v.push_back(s1.top().second);//push进vector进行计算
        s1.pop();
    }
    reverse(v.begin(), v.end());
    for (int i = 0; i < v.size(); i++)
    {
        sum += v[i];
    }
    cout << sum << endl;
    cout << v.size();
    for (int i = 0; i < v.size(); i++)
    {
            cout <<' '<< v[i];
    }
    cout<<endl;
}
```

