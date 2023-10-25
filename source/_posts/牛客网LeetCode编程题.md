
---
title: 牛客网LeetCode编程题
date: 2023-09-12 16:27:00
tags: Algorithm
categories: Algorithm
---

[牛客网LeetCode编程题](https://www.nowcoder.com/ta/classic-code)

##  二叉树的最小深度

描述
> 求给定二叉树的最小深度。最小深度是指树的根结点到最近叶子结点的最短路径上结点的数量
```
/**
 * struct TreeNode {
 *	int val;
 *	struct TreeNode *left;
 *	struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    int run(TreeNode* root) {
        if (root == nullptr) return 0;
        int left = run(root->left);
        int right = run(root->right);
        if (left == 0) return right + 1;
        else if (right == 0) return left + 1;
        else return min(left, right) + 1;
    }
};
```
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    int run(TreeNode* root) {
        if (root == nullptr) return 0;
        queue<TreeNode *> q;
        root->val = 1;
        q.push(root);
        while (!q.empty()) {
            TreeNode *cur = q.front();
            if (cur->left == nullptr && cur->right == nullptr) return cur->val;
            if (cur->left) {
               q.push(cur->left);
               cur->left->val = cur->val + 1; 
            }
            if (cur->right) {
                q.push(cur->right);
                cur->right->val = cur->val + 1;
            }
            q.pop();
        }
        return 0;
    }
};
```

## 后缀表达式求值

描述
> 计算逆波兰式（后缀表达式）的值
> 运算符仅包含"+","-","*"和"/"，被操作数是整数
> 保证表达式合法，除法时向下取整。

> 数据范围：表达式的长度满足： n<=1000
> 进阶：空间复杂度 O(n) 时间复杂度 O(n)
```
class Solution {
public:
    /**
     * 
     * @param tokens string字符串vector 
     * @return int整型
     */
    bool isOperator(string token) {
        return (token == "+" || token == "-" || token == "*" || token == "/");
    }
    int evalRPN(vector<string>& tokens) {
        int n = tokens.size();
        if (n == 0) return 0;
        stack<int> s;
        for (int i = 0; i < n; ++i) {
            if (!isOperator(tokens[i])) {
                s.push(stoi(tokens[i]));
            }
            else {
                int b = s.top();
                s.pop();
                int a = s.top();
                s.pop();
                if (tokens[i] == "+") s.push(a+b);
                if (tokens[i] == "-") s.push(a-b);
                if (tokens[i] == "*") s.push(a*b);
                if (tokens[i] == "/") s.push(a/b); 
            }
        }
        return s.top();
    }
};
```

## 多少个点位于同一直线

描述
> 对于给定的n个位于同一二维平面上的点，求最多能有多少个点位于同一直线上
```
/**
 * struct Point {
 *	int x;
 *	int y;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param points Point类vector 
     * @return int整型
     */
    int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
    int maxPoints(vector<Point>& points) {
        int n = points.size();
        if (n <= 2) return n;
        int max_points = 0;
        for (int i = 0; i < n; ++i) {
            map<pair<int, int>, int> mp;
            int dup = 1;
            for (int j = i + 1; j < n; ++j) {
                int x = points[j].x - points[i].x;
                int y = points[j].y - points[i].y;
                if (x == 0 && y == 0) dup++;
                else {
                    int g = gcd(x, y);
                    x /= g;
                    y /= g;
                    mp[{x, y}]++;
                }
            }
            max_points = max(max_points, dup);
            for (auto it = mp.begin(); it != mp.end(); ++it) {
                max_points = max(max_points, dup + it->second);
            }
        }
        return max_points;
    }
};
```
```
/**
 * struct Point {
 *  int x;
 *  int y;
 * };
 */

#include <cfloat>
class Solution {
public:
    /**
     * 
     * @param points Point类vector 
     * @return int整型
     */
    int maxPoints(vector<Point>& points) {
        int n = points.size();
        if (n <= 2) return n;
        int max_points = 0;
        for (int i = 0; i < n; ++i) {
            map<float, int> mp;
            int dup = 1;
            for (int j = i + 1; j < n; ++j) {
                int x = points[j].x - points[i].x;
                int y = points[j].y - points[i].y;
                if (x == 0 && y == 0) ++dup;
                else {
                    if (x == 0) mp[FLT_MAX]++;
                    else mp[(1.0 * y)/x]++; 
                }
            }
            max_points = max(max_points, dup);
            for (auto it = mp.begin(); it != mp.end(); ++it) {
                max_points = max(max_points, it->second + dup);
            }
        }
        return max_points;
    }
};
```

## 链表排序

描述
> 在O(n log n)的时间内使用常数级空间复杂度对链表进行排序
```
/**
 * struct ListNode {
 *  int val;
 *  struct ListNode *next;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param head ListNode类 
     * @return ListNode类
     */
    ListNode* merge(ListNode* h1, ListNode* h2) {
        if (h1 == nullptr) return h2;
        if (h2 == nullptr) return h1;
        if (h1->val < h2->val) {
            h1->next = merge(h1->next, h2);
            return h1;
        }
        else {
            h2->next = merge(h1, h2->next);
            return h2;
        }
    }
    ListNode* middle(ListNode* h) {
        if (h == nullptr) return nullptr;
        ListNode *slow = h, *fast = slow->next;
        while (slow && fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
    ListNode* sortList(ListNode* head) {
        if (head == nullptr || head->next == nullptr) return head;
        ListNode *m = middle(head);
        ListNode *h1 = head;
        ListNode *h2 = m->next;
        m->next = nullptr;
        h1 = sortList(h1);
        h2 = sortList(h2);
        return merge(h1, h2);
    }
};
```

## 链表的插入排序

描述
> 使用插入排序对链表进行排序
```
/**
 * struct ListNode {
 *  int val;
 *  struct ListNode *next;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param head ListNode类 
     * @return ListNode类
     */
    ListNode* insertionSortList(ListNode* head) {
        if (head == nullptr || head->next == nullptr) return head;
        ListNode *dummy = new ListNode(0);
        ListNode *cur = head, *nxt;
        while (cur) {
            nxt = cur->next;
            ListNode *pre = dummy;
            while (pre->next && pre->next->val < cur->val) pre = pre->next;
            cur->next = pre->next;
            pre->next = cur;
            cur = nxt;
        }
        return dummy->next;
    }
};
```

## 二叉树的后序遍历

描述
用递归的方法对给定的二叉树进行后序遍历
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return int整型vector
     */
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        if (root == nullptr) return res;
        stack<TreeNode *> s1, s2;
        s1.push(root);
        while (!s1.empty()) {
            TreeNode *cur = s1.top();
            s1.pop();
            if (cur->left) s1.push(cur->left);
            if (cur->right) s1.push(cur->right);
            s2.push(cur);
        }
        while (!s2.empty()) {
            TreeNode * cur = s2.top();
            res.push_back(cur->val);
            s2.pop();
        }
        return res;
    }
};
```

## 求二叉树的前序遍历

描述
> 求给定的二叉树的前序遍历
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return int整型vector
     */
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        if (root == nullptr) return res;
        stack<TreeNode *> s;
        TreeNode *cur = root;
        while (!s.empty() || cur) {
            while (cur) {
                res.push_back(cur->val);
                s.push(cur);
                cur = cur->left;
            }
            cur = s.top();
            s.pop();
            cur = cur->right;
        }
        return res;
    }
};
```

## 重排链表

描述
> 将给定的单链表L: L0->L1->...->Ln-1->Ln
> 重新排序为：L0->Ln->L1->Ln-1->...->
> 要求使用原地算法，不能只改变节点内部的值，需要对实际的节点进行交换

> 数据范围：链表长度 0 <= n <= 20000 链表中每个节点的值满足 0 <= val <= 1000

> 要求：空间复杂度 O(n) 并在链表上进行操作而不新建链表，时间复杂度 O(n)
> 进阶：空间复杂度 O(1) 时间复杂度 O(n)
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *reverse_list(ListNode *head) {
        ListNode *pre = nullptr, *cur = head, *nxt = nullptr;
        while (cur) {
            nxt = cur->next;
            cur->next = pre;
            pre = cur;
            cur = nxt;
        }
        return pre;
    }
    void reorderList(ListNode *head) {
        if (!head || !head->next) return;
        ListNode *slow = head, *fast = head->next;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode *l1 = head, *l2 = slow->next;
        slow->next = nullptr;
        l2 = reverse_list(l2);
        ListNode *p = l1, *q = l2, *t;
        while (q) {
            t = q->next;
            q->next = p->next;
            p->next = q;
            p = q->next;
            q = t;
        }
    }
};
```

## 链表中环的入口结点
  
描述
> 给一个长度为n链表，若其中包含环，请找出该链表的环的入口结点，否则，返回null
```
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead) {
        if (pHead == nullptr) return nullptr;
        ListNode *cur = pHead;
        map<ListNode *, int> mp;
        while (cur) {
            mp[cur]++;
            if (mp[cur] == 2) return cur;
            cur = cur->next;
        }
        return nullptr;
    }
};
```

## 判断链表中是否有环

描述
> 判断给定的链表中是否有环。如果有环则返回true，否则返回false
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (head == nullptr) return false;
        map<ListNode *, int> mp;
        while (head) {
            mp[head]++;
            if (mp[head] == 2) return true;
            head = head -> next;
        }
        return false;
    }
};
```

## 拆分词句

描述
> 给定一个字符串s和一组单词dict，判断s是否可以用空格分割成一个单词序列，使得单词序列中所有的单词都是dict中的单词（序列可以包含一个或多个单词）
> 例如:
> 给定s=“nowcode”
> dict=["now", "code"]
> 返回true，因为"nowcode"可以被分割成"now code"
```
class Solution {
public:
    bool wordBreak(string s, unordered_set<string> &dict) {
        int n = s.size();
        if (n == 0) return true;
        vector<bool> dp(n+1, false);
        dp[0] = true;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (dp[j] && dict.find(s.substr(j, i-j)) != dict.end()) dp[i] = true;
            }
        }
        return dp[n];  
    }
};
```

## 出现一次的数字ii

描述
> 现在有一个整数类型的数组，数组中只有一个元素只出现一次，其余元素都出现三次。你需要找出只出现一次的元素
> 数据范围： 数组长度满足 0 <= n <= 4000 数组中每个元素的值满足 0 <= val <= 2147483648
> 进阶: 空间复杂度 O(1) 时间复杂度 O(n)
```

class Solution {
public:
    /**
     * 
     * @param A int整型一维数组 
     * @param n int A数组长度
     * @return int整型
     */
    int singleNumber(int* A, int n) {
        if (!A || n == 0) return 0;
        int res = 0;
        for (int i = 0; i < 32; ++i) {
            int bit = 0;
            for (int j = 0; j < n; ++j) {
                bit += (A[j] >> i) & 1;
            }
            res += (bit % 3) << i;
        }
        return res;
    }
};
```

## 出现一次的数字

描述
> 现在有一个整数类型的数组，数组中素只有一个元素只出现一次，其余的元素都出现两次
> 数据范围：0 < n <= 4000  数组中每个值满足 0 <= val <= 4000
> 进阶： 空间复杂度 O(1) 时间复杂度 O(n)
```
class Solution {
public:
    /**
     * 
     * @param A int整型一维数组 
     * @param n int A数组长度
     * @return int整型
     */
    int singleNumber(int* A, int n) {
        if (n == 0) return -1;
        if (n == 1) return A[0];
        int res = 0;
        for (int i = 0; i < n; ++i) {
            res ^= A[i];
        }
        return res;
    }
};
```

## 分糖果

描述
> 有N个小朋友站在一排，每个小朋友都有一个评分
> 你现在要按以下的规则给孩子们分糖果：
> 每个小朋友至少要分得一颗糖果
> 分数高的小朋友要他比旁边得分低的小朋友分得的糖果多
> 你最少要分发多少颗糖果
```

class Solution {
public:
    /**
     * 
     * @param ratings int整型vector 
     * @return int整型
     */
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        if (n == 0) return 0;
        if (n == 1) return 1;
        vector<int> dp(n, 1);
        bool flag = true;
        while (flag) {
            flag = false;
            for (int i = 1; i < n; ++i) {
                if (ratings[i] > ratings[i-1] && dp[i] <= dp[i-1]) {
                    flag = true;
                    dp[i] = dp[i-1] + 1;   
                }
            }
            for (int j = n - 2; j >= 0; --j) {
                if (ratings[j] > ratings[j+1] && dp[j] <= dp[j+1]) {
                    flag = true;
                    dp[j] = dp[j+1] + 1;
                }
            }
        }
        int res = 0;
        for (int i = 0; i < n; ++i) {
            res += dp[i];
        }
        return res;
    }
};
```

## 加油站

描述
> 环形路上有n个加油站，第i个加油站的汽油量是gas[i].
> 你有一辆车，车的油箱可以无限装汽油。从加油站i走到下一个加油站（i+1）花费的油量是cost[i]，你从一个加油站出发，刚开始的时候油箱里面没有汽油
> 求从哪个加油站出发可以在环形路上走一圈。返回加油站的下标，如果没有答案的话返回-1
> 注意：答案保证唯一
```
class Solution {
public:
    /**
     * 
     * @param gas int整型vector 
     * @param cost int整型vector 
     * @return int整型
     */
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        if (n == 0) return -1;
        for (int p = 0; p < n; ++p) {
            int t = 0, g = 0;
            int i = p;
            while (t < n && i < n && (g + gas[i] - cost[i]) >= 0) {
                 g += (gas[i] - cost[i]);
                 ++i;
                 ++t;
                 if (i == n) i = 0;
            }
            if (t == n) return p;
        }
        return -1;
    }
};
```
```
class Solution {
public:
    /**
     * 
     * @param gas int整型vector 
     * @param cost int整型vector 
     * @return int整型
     */
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        if (n == 0) return -1;
        int sum = 0, cur = 0, idx = -1;
        for (int i = 0; i < n; ++i) {
            sum += (gas[i] - cost[i]);
            cur += (gas[i] - cost[i]);
            if (cur < 0) {
                cur = 0;
                idx = i;
            }
        }
        return sum >= 0 ? idx + 1 : -1;
    }
};
```

## 复制无向图

描述
> 本题要求复制一个无向图，图中每个节点都包含一个标签和它的邻居列表
```
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
public:
    void dfs(UndirectedGraphNode *node, map<UndirectedGraphNode *, UndirectedGraphNode *> &mp) {
        if (node == nullptr) return;
        if (mp[node]) return;
        mp[node] = new UndirectedGraphNode(node->label);
        for (auto it : node->neighbors) {
            dfs(it, mp);
        }
    }
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if (node == nullptr) return nullptr;
        map<UndirectedGraphNode *, UndirectedGraphNode *> mp;
        dfs(node, mp);
        for (auto it = mp.begin(); it != mp.end(); ++it) {
            for (auto cur : it->first->neighbors) {
                it->second->neighbors.push_back(cur);
            }
        }
        return mp[node];
    }
};
```

## 分割回文串-ii

描述
> 给出一个字符串s，分割s使得分割出的每一个子串都是回文串
> 计算将字符串s分割成回文分割结果的最小切割数
> 例如:给定字符串s="aab",
> 返回1，因为回文分割结果["aa","b"]是切割一次生成的
```
class Solution {
public:
    /**
     * 
     * @param s string字符串 
     * @return int整型
     */
    int minCut(string s) {
        int n = s.size();
        if (n == 0) return 0;
        
        vector<vector<bool> > path(n, vector<bool>(n, false));
        for (int i = 0; i < n; ++i) path[i][i] = true;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (s[j] == s[i] && i - j < 2) path[j][i] = true;
                else if (s[j] == s[i] && i - j >=2 ) path[j][i] = path[j+1][i-1];
                else continue;
            }
        }

        vector<int> dp(n+1, INT_MAX);
        dp[0] = -1;
        for (int i = 1; i <= n; ++i) {
            for (int j = i-1; j >= 0; --j) {
                if (path[j][i-1]) {
                    dp[i] = min(dp[i], dp[j] + 1);
                }
            }
        }
        
        return dp[n];
    }
};
```

## 分割回文串

描述
> 给定一个字符串s，分割s使得s的每一个子串都是回文串
> 返回所有的回文分割结果。（注意：返回结果的顺序需要和输入字符串中的字母顺序一致
```
class Solution {
public:
    /**
     * 
     * @param s string字符串 
     * @return string字符串vector<vector<>>
     */
    bool is_palindrome(string s) {
        // int n = s.size();
        // if (n <= 1) return true;
        // for (int i = 0, j = n-1; i <= j; ++i, --j) {
        //     if (s[i] != s[j]) return false;
        // }
        // return true;
        return (s == string(s.rbegin(), s.rend()));
    }
    void dfs(vector<vector<string> > &res, vector<string> &path, string s, int index) {
        int n = s.size();
        if (n == index) {
            res.push_back(path);
            return;
        }
        for (int i = 1; i <= n-index; ++i) {
            if (is_palindrome(s.substr(index, i))) {
                path.push_back(s.substr(index, i));
                dfs(res, path, s, index+i);
                path.pop_back();
            }
        }

    }
    vector<vector<string> > partition(string s) {
        int n = s.size();
        vector<vector<string> > res;
        vector<string> path;
        if (n == 0) return res;
        dfs(res, path, s, 0);
        return res;
    }
};
```

## 包围区域

描述
> 现在有一个仅包含‘X’和‘O’的二维板，请捕获所有的被‘X’包围的区域
> 捕获一个被包围区域的方法是将被包围区域中的所有‘O’变成‘X’
```
class Solution {
public:
    void dfs(vector<vector<char> > &board, int i, int j) {
        int m = board.size();
        if (m == 0) return;
        int n = board[0].size();
        if (i < 0 || j < 0 || i >= m || j >= n) return;
        if (board[i][j] != 'O') return;
        board[i][j] = 'A';
        dfs(board, i+1, j);
        dfs(board, i-1, j);
        dfs(board, i, j-1);
        dfs(board, i, j+1);
    }
    void solve(vector<vector<char>> &board) {
        int m = board.size();
        if (m == 0) return;
        int n = board[0].size();
        for (int i = 0; i < m; ++i) {
            dfs(board, i, 0);
            dfs(board, i, n-1);
        }
        for  (int j = 0; j < n; ++j) {
            dfs(board, 0, j);
            dfs(board, m-1, j);
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 'O') board[i][j] = 'X';
                if (board[i][j] == 'A') board[i][j] = 'O';
            }
        }
    }
};

```

## 二叉树根节点到叶子节点的所有路径和

描述
> 给定一个二叉树的根节点root，该树的节点值都在数字0−9 之间，每一条从根节点到叶子节点的路径都可以用一个数字表示
> 1.该题路径定义为从树的根结点开始往下一直到叶子结点所经过的结点
> 2.叶子节点是指没有子节点的节点
> 3.路径只能从父节点到子节点，不能从子节点到父节点
> 4.总节点数目为n
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    void dfs(vector<vector<int> > &res, vector<int> &path, TreeNode *root) {
        if (root == nullptr) return;
        path.push_back(root->val);
        dfs(res, path, root->left);
        dfs(res, path, root->right);
        if (root->left == nullptr && root->right == nullptr) res.push_back(path);
        path.pop_back();
    }
    int sumNumbers(TreeNode* root) {
        vector<vector<int> > res;
        vector<int> path;
        dfs(res, path, root);
        int n = res.size();
        if (n == 0) return 0;
        int total = 0;
        for (int i = 0; i < n; ++i) {
            int m = res[i].size();
            int sum = 0;
            for (int j = 0; j < m; ++j) {
                sum = 10 * sum + res[i][j];
            }
            total += sum;
        }
        return total;
    }
};
```
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    void dfs(TreeNode *root, int path, int &sum) {
        if (!root) return;
        path = 10 * path + root->val;
        if (root->left == nullptr && root->right == nullptr) {
            sum += path;
        } 
        dfs(root->left, path, sum);
        dfs(root->right, path, sum);
        
//         path = path / 10;
    }
   
    int sumNumbers(TreeNode* root) {
        int sum = 0;
        int path = 0;
        dfs(root, path, sum);
        return sum;
    }
};
```

## 最长的连续元素序列长度

描述
> 给定一个无序的整数类型数组，求最长的连续元素序列的长度
> 例如：
> 给出的数组为[1000, 4, 2000, 1, 3, 2],
> 最长的连续元素序列为[1, 2, 3, 4]. 返回这个序列的长度：4
> 你需要给出时间复杂度在O（n）之内的算法
```
class Solution {
public:
    /**
     * 
     * @param num int整型vector 
     * @return int整型
     */
    int longestConsecutive(vector<int>& num) {
        int n = num.size();
        if (n == 0) return 0;
        map<int, int> mp;
        for (int i = 0; i < n; ++i) mp[num[i]]++;
        int res = 0;
        for (auto it = mp.begin(); it != mp.end(); ++it) {
            if (mp[it->first] > 0) {
                int l = 1;
                int i = it->first + 1;
                while (mp[i] >= 1) {
                    ++l;
                    mp[i] = 0;
                    ++i;
                }
                i = it->first - 1;
                while (mp[i] >= 1) {
                    ++l;
                    mp[i] = 0;
                    --i;
                }
                mp[it->first] = 0;
                res = max(res, l);
            }
        }
        return res;
    }
};
```

## 词语序列

描述
> 给定两个单词（初始单词和目标单词）和一个单词字典，请找出所有的从初始单词到目标单词的最短转换序列的长度
> 每一次转换只能改变一个单词
> 每一个中间词都必须存在单词字典当中
```
class Solution {
public:
    int ladderLength(string start, string end, unordered_set<string> &dict) {
        queue<string> q;
        unordered_set<string> s;
        q.push(start);
        s.emplace(start);
        int res = 1;
        while (!q.empty()) {
            int q_size = q.size();
            while (q_size--) {
                string cur = q.front();
                q.pop();
                if (cur == end) return res;
                int n = cur.size();
                for (int i = 0; i < n; ++i) {
                    string nxt(cur);
                    for (int j = 'a'; j <= 'z'; ++j) {
                        if (nxt[i] == j) continue;
                        nxt[i] = j;
                        // if (s.find(nxt) != s.end() || dict.find(nxt) == dict.end()) continue;
                        if (s.count(nxt) || !dict.count(nxt)) continue;
                        s.emplace(nxt);
                        q.push(nxt);
                    }
                }
                
            }
            ++res;
        }
        return 0;
    }
};
```

## 判断回文串

描述
> 判断题目给出的字符串是不是回文，仅考虑字符串中的字母字符和数字字符，并且忽略大小写
> 例如："nowcoder Is Best tsebsi: redoc won"是回文  "race a car"不是回文
注意：
> 你有没有考虑过字符串可能为空？这是面试时应该提出的一个好问题
> 针对这个问题，我们定义空字符串是回文
```
class Solution {
public:
    /**
     * 
     * @param s string字符串 
     * @return bool布尔型
     */
    bool isPalindrome(string s) {
        int n = s.size();
        if (n == 0) return true;
        int index = 0;
        for (int i = 0; i < n; ++i) {
            if (s[i] >= 'A' && s[i] <= 'Z') {
                s[index++] = s[i] - 'A' + 'a';
            }
            else if ((s[i] >= 'a' && s[i] <= 'z') || (s[i] >= '0' && s[i] <= '9')) {
                s[index++] = s[i];
            }
            else continue;
        }
        string a = s.substr(0, index);
        return a == string(a.rbegin(), a.rend());
    }
};
```

## 二叉树中的最大路径和

> 描述
> 二叉树里面的路径被定义为:从该树的任意节点出发，经过父=>子或者子=>父的连接，达到任意节点的序列
> 注意:
> 1.同一个节点在一条二叉树路径里中最多出现一次
> 2.一条路径至少包含一个节点，且不一定经过根节点

> 给定一个二叉树的根节点root，请你计算它的最大路径和
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    int dfs(TreeNode *root, int &ans) {
        if (!root) return 0;
        int left_sum = max(dfs(root->left, ans), 0);
        int right_sum = max(dfs(root->right, ans), 0);
        ans = max(ans, left_sum + right_sum + root->val);
        return max(left_sum, right_sum) + root->val;
    }
    int maxPathSum(TreeNode* root) {
        if (!root) return 0;
        int ans = INT_MIN;
        dfs(root, ans);
        return ans;
    }
};
```

## 买卖股票的最好时机 iii

描述
> 假设你有一个数组，其中第i个元素是某只股票在第i天的价格
> 设计一个算法来求最大的利润。你最多可以进行两次交易
> 注意:
> 你不能同时进行多个交易(即，你必须在再次购买之前出售之前买的股票)
```
class Solution {
public:
    /**
     * 
     * @param prices int整型vector 
     * @return int整型
     */
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 0) return 0;
        vector<vector<int> > dp(n, vector<int>(5, 0));
        dp[0][1] = -prices[0];
        dp[0][2] = 0;
        dp[0][3] = -prices[0];
        dp[0][4] = 0;
        for (int i = 1; i < n; ++i) {
            dp[i][0] = 0;
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]);
            dp[i][2] = max(dp[i-1][2], dp[i-1][1] + prices[i]);
            dp[i][3] = max(dp[i-1][3], dp[i-1][2] - prices[i]);
            dp[i][4] = max(dp[i-1][4], dp[i-1][3] + prices[i]);
        }
        return dp[n-1][4];
    }
};
```

## 买卖股票的最好时机 ii

描述
> 假设你有一个数组，其中第i个元素表示某只股票在第i天的价格。
> 设计一个算法来寻找最大的利润。你可以完成任意数量的交易(例如，多次购买和出售股票的一股)。但是，你不能同时进行多个交易(即，你必须在再次购买之前卖出之前买的股票)
```
class Solution {
public:
    /**
     * 
     * @param prices int整型vector 
     * @return int整型
     */
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 0 || n == 1) return 0;
        int max_profit = 0;
        for (int i = 1; i < n; ++i) {
            if (prices[i] > prices[i-1]) max_profit += prices[i] - prices[i-1];
        }
        return max_profit;
    }
};
```

## 买卖股票的最好时机

描述
> 假设你有一个数组prices，长度为n，其中prices[i]是股票在第i天的价格，请根据这个价格数组，返回买卖股票能获得的最大收益
> 1.你可以买入一次股票和卖出一次股票，并非每天都可以买入或卖出一次，总共只能买入和卖出一次，且买入必须在卖出的前面的某一天
> 2.如果不能获取到任何利润，请返回0
> 3.假设买入卖出均无手续费
```
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param prices int整型vector 
     * @return int整型
     */
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 0) return 0;
        int min_value = prices[0];
        int max_profit = 0;
        for (int i = 1; i < n; ++i) {
            max_profit = max(max_profit, prices[i] - min_value);
            min_value = min(min_value, prices[i]);
        }
        return max_profit;
    }
};
```

## 三角形

描述
> 给出一个三角形，计算从三角形顶部到底部的最小路径和，每一步都可以移动到下面一行相邻的数字
```
class Solution {
public:
    int minimumTotal(vector<vector<int> > &triangle) {
        int n = triangle.size();
        if (n == 0) return 0;
        vector<vector<int> > dp(n, vector<int>(n, 0));
        dp[0][0] = triangle[0][0];
        for (int j = 1; j < n; ++j) {
            dp[j][0] = dp[j-1][0] + triangle[j][0];
            dp[j][j] = dp[j-1][j-1] + triangle[j][j];
        }
        for (int i = 1; i < n; ++i) {
            for (int j = 1; j < i; ++j) {
                dp[i][j] = min(dp[i-1][j], dp[i-1][j-1]) + triangle[i][j];
            }
        }
        return *min_element(dp[n-1].begin(), dp[n-1].end());
    }
};
```
```
class Solution {
public:
    int minimumTotal(vector<vector<int> > &triangle) {
        int n = triangle.size();
        if (n == 0) return 0;
        for (int i = n - 2; i >= 0; --i) {
            for (int j = 0; j <= i; ++j) {
                triangle[i][j] += min(triangle[i+1][j], triangle[i+1][j+1]);
            }
        }
        return triangle[0][0];
    }
};
```

## 杨辉三角-ii

描述
> 给出一个索引k，返回杨辉三角的第k行
> 例如，k=3，
> 返回[1,3,3,1].
> 备注：
> 你能将你的算法优化到只使用O(k)的额外空间吗?
```
class Solution {
public:
    /**
     * 
     * @param rowIndex int整型 
     * @return int整型vector
     */
    vector<int> getRow(int rowIndex) {
        int n = rowIndex;
        vector<vector<int> > dp(n+1, vector<int>(n+1, 1));
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j < i; ++j) {
                dp[i][j] = dp[i-1][j-1]+dp[i-1][j];
            }
        }
        return dp[n];
    }
};
```
```
class Solution {
public:
    /**
     * 
     * @param rowIndex int整型 
     * @return int整型vector
     */
    vector<int> getRow(int rowIndex) {
        int n = rowIndex;
        vector<int> a(n+1, 1);
        vector<int> b(n+1, 1);
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j < i; ++j) a[j] = b[j] + b[j-1];
            for (int j = 1; j < i; ++j) b[j] = a[j];

        }
        return b;
    }
};
```

## 杨辉三角

描述
> 给出一个值numRows，生成杨辉三角的前numRows行
```
class Solution {
public:
    /**
     * 
     * @param numRows int整型 
     * @return int整型vector<vector<>>
     */
    vector<vector<int> > generate(int numRows) {
        int n = numRows;
        vector<vector<int> > dp(n, vector<int>(n, 1));
        for (int i = 2; i < n; ++i) {
            for (int j = 1; j < i; ++j) {
                dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
            }
        }
        vector<vector<int> > res;
        for (int i = 0; i < n; ++i) {
            vector<int> tmp;
            for (int j = 0; j <= i; ++j) {
                tmp.push_back(dp[i][j]);
            }
            res.push_back(tmp);
        }
        return res;
    }
};
```

## 填充每个节点指向最右节点的next指针 ii
描述
> 继续思考"填充每个节点指向最右节点的next指针" 这道题
> 如果给定的树可以是任意的二叉树呢?你之前的给出的算法还有效吗?
> 注意：
> 你只能使用常量的额外内存空间
```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if (root == nullptr) return;
        queue<TreeLinkNode *> q1;
        queue<TreeLinkNode *> q2;
        q1.push(root);
        TreeLinkNode *nxt, *cur;
        while (!q1.empty() || !q2.empty()) {
            while (!q1.empty()) {
                cur = q1.front();
                q1.pop();
                if (!q1.empty()) nxt = q1.front();
                else nxt = nullptr;
                cur->next = nxt;
                if (cur->left) q2.push(cur->left);
                if (cur->right) q2.push(cur->right);
            }
            while (!q2.empty()) {
                cur = q2.front();
                q2.pop();
                if (!q2.empty()) nxt = q2.front();
                else nxt = nullptr;
                cur->next = nxt;
                if (cur->left) q1.push(cur->left);
                if (cur->right) q1.push(cur->right);
            }
        }        
    }
};
```
```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if (root == nullptr) return;
        queue<TreeLinkNode *> q;
        q.push(root);
        TreeLinkNode *cur;
        while (!q.empty()) {
            int n = q.size();
            while (n--) {
                cur = q.front();
                q.pop();
                if (n == 0) cur->next = nullptr;
                else cur->next = q.front();
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
        }        
    }
};
```

## 填充每个节点指向最右节点的next指针

描述
> 给定一个二叉树
> 填充所有节点的next指针，指向最接近它的同一层右边节点。如果没有同一层没有右边的节点，则应该将next指针设置为NULL
> 初始时，所有的next指针都为NULL
> 注意：
> 你只能使用常量级的额外内存空间
> 可以假设给出的二叉树是一个完美的二叉树(即，所有叶子节点都位于同一层，而且每个父节点都有两个孩子节点)
```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if (!root) return;
        TreeLinkNode *level = root;
        TreeLinkNode *cur;
        while (level) {
            cur = level;
            while (cur) {
                if (cur->left) cur->left->next = cur->right;
                if (cur->next && cur->right) cur->right->next = cur->next->left;
                cur = cur->next;
            }
            level = level->left;
        }
    }
};
```

## 不同的子序列

描述
> 给定两个字符串S和T，返回S子序列等于T的不同子序列个数有多少个？
> 字符串的子序列是由原来的字符串删除一些字符（也可以不删除）在不改变相对位置的情况下的剩余字符（例如，"ACE"is a subsequence of"ABCDE"但是"AEC"不是）
> 例如：
> S="nowcccoder", T = "nowccoder"
> 返回3
```
class Solution {
public:
    /**
     * 
     * @param S string字符串 
     * @param T string字符串 
     * @return int整型
     */
    int numDistinct(string S, string T) {
        int n = S.size();
        int m = T.size();
        vector<vector<int> > dp(n+1, vector<int>(m+1, 0));
        for (int i = 0; i <= n; ++i) dp[i][0] = 1;
        for (int j = 1; j <= m; ++j) dp[0][j] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= m; ++j) {
                if (S[i-1] != T[j-1]) dp[i][j] = dp[i-1][j];
                else dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
            }
        }
        return dp[n][m];
    }
};
```

## 二叉树中和为某一值的路径(二)

描述
> 输入一颗二叉树的根节点root和一个整数expectNumber，找出二叉树中结点值的和为expectNumber的所有路径
> 1.该题路径定义为从树的根结点开始往下一直到叶子结点所经过的结点
> 2.叶子节点是指没有子节点的节点
> 3.路径只能从父节点到子节点，不能从子节点到父节点
> 4.总节点数目为n
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param target int整型 
     * @return int整型vector<vector<>>
     */
    void dfs(TreeNode *root, vector<vector<int> > &res, vector<int> &path, int &sum, int target) {
        if (!root) return;
        sum += root->val;
        path.push_back(root->val);
        if (!root->left && !root->right && sum == target) res.push_back(path);
        dfs(root->left, res, path, sum, target);
        dfs(root->right, res, path, sum, target);

        path.pop_back();
        sum -= root->val;
    }
    vector<vector<int> > FindPath(TreeNode* root, int target) {
        vector<vector<int> > res;
        vector<int> path;
        int sum = 0;
        dfs(root, res, path, sum, target);
        return res;
    }
};
```

## 二叉树中和为某一值的路径(一)

描述
> 给定一个二叉树root和一个值 sum ，判断是否有从根节点到叶子节点的节点值之和等于 sum 的路径
> 1.该题路径定义为从树的根结点开始往下一直到叶子结点所经过的结点
> 2.叶子节点是指没有子节点的节点
> 3.路径只能从父节点到子节点，不能从子节点到父节点
> 4.总节点数目为n
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return bool布尔型
     */
    bool hasPathSum(TreeNode* root, int sum) {
        if (root && !root->left && !root->right && root->val == sum) return true;
        if (!root) return false;
        return hasPathSum(root->left, sum-root->val) || hasPathSum(root->right, sum-root->val);
    }
};
```
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return bool布尔型
     */
    bool dfs(TreeNode *root, int sum) {
        if (!root) return false;
        if (root && !root->left && !root->right && root->val == sum) return true;
        return dfs(root->left, sum-root->val) || dfs(root->right, sum-root->val);
    }
    bool hasPathSum(TreeNode* root, int sum) {
        return dfs(root, sum);
    }
};
```

## 判断二叉树是否为平衡二叉树

描述
> 本题要求判断给定的二叉树是否是平衡二叉树
> 平衡二叉树的性质为: 要么是一棵空树，要么任何一个节点的左右子树高度差的绝对值不超过 1
> 一颗树的高度指的是树的根节点到所有节点的距离中的最大值
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return bool布尔型
     */
    int depth(TreeNode *root) {
        if (!root) return 0;
        int left = depth(root->left);
        int right = depth(root->right);
        return max(left, right) + 1;
    }
    bool isBalanced(TreeNode* root) {
        if (!root) return true;
        int left = depth(root->left);
        int right = depth(root->right);

        return isBalanced(root->left) && isBalanced(root->right) && abs(left-right) <= 1; 
    }
};
```

## 有序链表变成二叉搜索树

描述
> 给定一个单链表，其中的元素按升序排序，请将它转化成平衡二叉搜索树（BST）
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */
/**
 * struct ListNode {
 *  int val;
 *  struct ListNode *next;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param head ListNode类 
     * @return TreeNode类
     */
    TreeNode *merge(ListNode *head, ListNode *tail) {
        if (head == tail) return nullptr;
        ListNode *slow = head, *fast = head;
        while (fast != tail && fast->next != tail) {
            slow = slow->next;
            fast = fast->next->next;
        }
        TreeNode *root = new TreeNode(slow->val);
        root->left = merge(head, slow);
        root->right = merge(slow->next, tail);
        return root;
    }
    TreeNode* sortedListToBST(ListNode* head) {
        return merge(head, nullptr);
    }
};
```

## 将升序数组转化为平衡二叉搜索树

描述
> 给定一个升序排序的数组，将其转化为平衡二叉搜索树（BST）
> 平衡二叉搜索树指树上每个节点 node 都满足左子树中所有节点的的值都小于 node 的值，右子树中所有节点的值都大于 node 的值，并且左右子树的节点数量之差不大于1
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型vector 
     * @return TreeNode类
     */
    TreeNode *merge(vector<int> &nums, int left, int right) {
        if (left == right) return nullptr;
        int mid = left + (right - left) / 2;
        TreeNode *root = new TreeNode(nums[mid]);
        root->left = merge(nums, left, mid);
        root->right = merge(nums, mid+1, right);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int n = nums.size();
        return merge(nums, 0, n);
    }
};
```

## 二叉树层序遍历 ii

描述
> 给定一个二叉树，返回该二叉树由底层到顶层的层序遍历，（从左向右，从叶子节点到根节点，一层一层的遍历）
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return int整型vector<vector<>>
     */
    vector<vector<int> > levelOrderBottom(TreeNode* root) {
        vector<vector<int> > res;
        if (!root) return res;
        queue<TreeNode *> q;
        q.push(root);
        TreeNode *cur;
        while (!q.empty()) {
            int n = q.size();
            vector<int> tmp;
            while (n--) {
                cur = q.front();
                tmp.push_back(cur->val);
                q.pop();
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            res.push_back(tmp);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

## 从中序和后序遍历构造二叉树

描述
> 给出一棵树的中序遍历和后序遍历，请构造这颗二叉树
> 注意：
> 保证给出的树中不存在重复的节点
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param inorder int整型vector 
     * @param postorder int整型vector 
     * @return TreeNode类
     */
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int n = inorder.size();
        if (n == 0) return nullptr;
        if (n == 1) return new TreeNode(inorder[0]);

        int index = 0;
        for (int i = 0; i < n; ++i) {
            if (inorder[i] == postorder[n-1]) {
                index = i;
                break;
            }
        }

        vector<int> leftInorder, leftPostorder, rightInorder, rightPostorder;
        for (int i = 0; i < index; ++i) leftInorder.push_back(inorder[i]);
        for (int i = index + 1; i < n; ++i) rightInorder.push_back(inorder[i]);
        for (int i = index; i < n - 1; ++i) rightPostorder.push_back(postorder[i]);
        for (int i = 0; i < index; ++i) leftPostorder.push_back(postorder[i]);

        TreeNode *root = new TreeNode(inorder[index]);

        root->left =  buildTree(leftInorder, leftPostorder);
        root->right = buildTree(rightInorder, rightPostorder);

        return root;
    }
};
```

## 从前序和中序遍历构造二叉树

描述
> 给出一棵树的前序遍历和中序遍历，请构造这颗二叉树
> 注意：
> 可以假设树中不存在重复的节点
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param preorder int整型vector 
     * @param inorder int整型vector 
     * @return TreeNode类
     */
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        if (n == 0) return nullptr;
        if (n == 1) return new TreeNode(preorder[0]);

        int index = 0;
        for (int i = 0; i < n; ++i) {
            if (inorder[i] == preorder[0]) {
                index = i;
                break;
            }
        }

        vector<int> leftPreorder, leftInorder, rightPreorder, rightInorder;
        TreeNode *root = new TreeNode(inorder[index]);

        for (int i = 0; i < index; ++i) leftInorder.push_back(inorder[i]);
        for (int i = index+1; i < n; ++i) rightInorder.push_back(inorder[i]);
        for (int i = 1; i <= index; ++i) leftPreorder.push_back(preorder[i]);
        for (int i = index+1; i < n; ++i) rightPreorder.push_back(preorder[i]);

        root->left = buildTree(leftPreorder, leftInorder);
        root->right = buildTree(rightPreorder, rightInorder);

        return root;
    }
};
```

## 二叉树的最大深度

描述
> 求给定二叉树的最大深度
> 深度是指树的根节点到任一叶子节点路径上节点的数量
> 最大深度是所有叶子节点的深度的最大值
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

## 按之字形顺序打印二叉树

描述
> 给定一个二叉树，返回该二叉树的之字形层序遍历，（第一层从左向右，下一层从右向左，一直这样交替）
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pRoot TreeNode类 
     * @return int整型vector<vector<>>
     */
    vector<vector<int> > Print(TreeNode* pRoot) {
        vector<vector<int> > res;
        if (!pRoot) return res;
        queue<TreeNode *> q;
        q.push(pRoot);
        TreeNode *cur;
        int level = 1;
        while (!q.empty()) {
            vector<int> tmp;
            int n = q.size();
            while (n--) {
                cur = q.front();
                tmp.push_back(cur->val);
                q.pop();
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            if (level & 1) res.push_back(tmp);
            else {
                reverse(tmp.begin(), tmp.end());
                res.push_back(tmp);
            }
            ++level;
        }
        return res;
    }
};
```
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pRoot TreeNode类 
     * @return int整型vector<vector<>>
     */
    vector<vector<int> > Print(TreeNode* pRoot) {
        vector<vector<int> > res;
        if (!pRoot) return res;
        stack<TreeNode *> s1, s2;
        s1.push(pRoot);
        TreeNode *cur;
        while (!s1.empty() || !s2.empty()) {
            vector<int> tmp;
            while (!s1.empty()) {
                cur = s1.top();
                tmp.push_back(cur->val);
                s1.pop();
                if (cur->left) s2.push(cur->left);
                if (cur->right) s2.push(cur->right);
            }
            if (!tmp.empty()) res.push_back(tmp);
            tmp.clear();
            while (!s2.empty()) {
                cur = s2.top();
                tmp.push_back(cur->val);
                s2.pop();
                if (cur->right) s1.push(cur->right);
                if (cur->left) s1.push(cur->left);
            }
            if (!tmp.empty()) res.push_back(tmp);
        }
        return res;
    }
};
```

## 求二叉树的层序遍历

描述
> 给定一个二叉树，返回该二叉树层序遍历的结果，（从左到右，一层一层地遍历）
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型vector<vector<>>
     */
    vector<vector<int> > levelOrder(TreeNode* root) {
        vector<vector<int> > res;
        if (!root) return res;
        queue<TreeNode *> q;
        q.push(root);
        TreeNode *cur;
        while (!q.empty()) {
            int n = q.size();
            vector<int> tmp;
            while (n--) {
                cur = q.front();
                tmp.push_back(cur->val);
                q.pop();
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            res.push_back(tmp); 
        }
        return res;
    }
};
```

## 对称的二叉树

描述
> 给定一棵二叉树，判断其是否是自身的镜像（即：是否对称）
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 *  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pRoot TreeNode类 
     * @return bool布尔型
     */
    bool same(TreeNode *r1, TreeNode *r2) {
        return (!r1 && !r2) || (r1 && r2 && r1->val == r2->val && same(r1->left, r2->left) && same(r1->right, r2->right));
    }
    void mirror(TreeNode *root) {
        if (!root || (!root->left && !root->right)) return;
        TreeNode *left = root->left;
        TreeNode *right = root->right;
        mirror(left);
        mirror(right);
        root->left = right;
        root->right = left;
    }
    bool isSymmetrical(TreeNode* pRoot) {
        if (!pRoot) return true;
        mirror(pRoot->right);
        return same(pRoot->left, pRoot->right);
    }
};
```

## 判断二叉树是否相等

描述
> 给出两个二叉树，请写出一个判断两个二叉树是否相等的函数
> 判断两个二叉树相等的条件是：两个二叉树的结构相同，并且相同的节点上具有相同的值
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param p TreeNode类 
     * @param q TreeNode类 
     * @return bool布尔型
     */
    bool isSameTree(TreeNode* p, TreeNode* q) {
        return (!p && !q) || (p && q && p->val == q->val && isSameTree(p->left, q->left) && isSameTree(p->right, q->right));
    }
};
```

## 恢复二叉搜索树

描述
> 二叉搜索树（BST）中的两个节点的值被错误地交换了
> 请在不改变树的结构的情况下恢复这棵树
> 备注；
> 用O(n)的空间解决这个问题的方法太暴力了，你能设计一个常数级空间复杂度的算法么？
```
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    void inorder(TreeNode *root, TreeNode *&pre, TreeNode *&first, TreeNode *&second) {
        if (!root) return;
        inorder(root->left, pre, first, second);
        if (pre && pre->val > root->val) {
            if (!first) first = pre;
            second = root;
        }
        pre = root;
        inorder(root->right, pre, first, second);
       
    }
    void recoverTree(TreeNode *root) {
        TreeNode *pre = nullptr, *first = nullptr, *second = nullptr;
        inorder(root, pre, first, second);
        if (first && second) swap(first->val, second->val);
    }
};
```
```
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    TreeNode *pre = nullptr, *first = nullptr, *second = nullptr;
public:
    void inorder(TreeNode *root) {
        if (!root) return;
        inorder(root->left);
        if (pre && pre->val > root->val) {
            if (!first) first = pre;
            second = root;
        }
        pre = root;
        inorder(root->right);
       
    }
    void recoverTree(TreeNode *root) {
        inorder(root);
        if (first && second) swap(first->val, second->val);
    }
};
```

## 判断二叉搜索树

描述
> 判断给出的二叉树是否是一个二叉搜索树（BST）
> 二叉搜索树的定义如下
> 一个节点的左子树上节点的值都小于自身的节点值
> 一个节点的右子树上节点的值都大于自身的节点值
> 所有节点的左右子树都必须是二叉搜索树
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
private:
    TreeNode *pre = nullptr;
    bool flag = true;
public:
    /**
     * 
     * @param root TreeNode类 
     * @return bool布尔型
     */
    void inorder(TreeNode *root) {
        if (!root) return;
        inorder(root->left);
        if (pre && pre->val >= root->val) flag = false;
        pre = root;
        inorder(root->right);
    }
    bool isValidBST(TreeNode* root) {
        inorder(root);
        return flag;
    }
};
```

## 交织的字符串

描述
> 给出三个字符串s1, s2, s3,判断s3是否可以由s1和s2交织而成
> 例如：
> 给定
> s1 ="xxyzz",
> s2 ="pyyzx",
> 如果s3 ="xxpyyzyzxz", 返回true
> 如果s3 ="xxpyyyxzzz", 返回false
```
class Solution {
public:
    /**
     * 
     * @param s1 string字符串 
     * @param s2 string字符串 
     * @param s3 string字符串 
     * @return bool布尔型
     */
    bool isInterleave(string s1, string s2, string s3) {
        int n = s1.size();
        int m = s2.size();
        int l = s3.size();
        if (l != n + m) return false;
        vector<vector<bool>> dp(n+1, vector<bool>(m+1, false));
        dp[0][0] = true;
        for (int i = 0; i < n; ++i) {
            if (s1[i] == s3[i]) dp[i+1][0] = true;
        }
        for (int j = 0; j < m; ++j) {
            if (s2[j] == s3[j]) dp[0][j+1] = true;
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                char a = s1[i], b = s2[j], c = s3[i+j+1];
                if (a == c && b != c) dp[i+1][j+1] = dp[i][j+1];
                if (a != c && b == c) dp[i+1][j+1] = dp[i+1][j];
                if (a == c && b == c) dp[i+1][j+1] = dp[i][j+1] || dp[i+1][j];
                if (a != c && b != c) dp[i+1][j+1] = false;
            }
        }
        return dp[n][m];
    }
};
```

## 不同的二叉搜索树 ii

描述
> 给定一个值n,请生成所有的存储值1...n.的二叉搜索树（BST）的结构
> 例如：
> 给定n=3，你的程序应该给出下面五种不同的二叉搜索树（BST）
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param n int整型 
     * @return TreeNode类vector
     */
    vector<TreeNode *> postorder(int left, int right) {
        if (left > right) return vector<TreeNode *>{nullptr};
        vector<TreeNode *> res;
        for (int i = left; i <= right; ++i) {
            vector<TreeNode *> left_vec = postorder(left, i-1);
            vector<TreeNode *> right_vec = postorder(i+1, right);
            int n = left_vec.size();
            int m = right_vec.size();
            for (int a = 0; a < n; ++a) {
                for (int b = 0; b < m; ++b) {
                    TreeNode *root = new TreeNode(i);
                    root->left = left_vec[a];
                    root->right = right_vec[b];
                    res.push_back(root);
                }
            }
        }
        return res;
    }
    vector<TreeNode*> generateTrees(int n) {
        return postorder(1, n);
    }
};
```

## 不同的二叉搜索树

描述
> 给定一个值n，能构建出多少不同的值包含1...n的二叉搜索树（BST）？
> 例如
> 给定 n = 3, 有五种不同的二叉搜索树（BST）
```
class Solution {
public:
    /**
     * 
     * @param n int整型 
     * @return int整型
     */
    int postorder(int left, int right) {
        if (left > right) return 0;
        int res = 0;
        for (int i = left; i <= right; ++i) {
            int l = postorder(left, i-1);
            int r = postorder(i+1, right);
            if (l != 0 && r != 0) res += l * r;
            else if (l == 0 && r == 0) res += 1;
            else if (l == 0) res += r;
            else res += l;
        }
        return res;
    }
    int numTrees(int n) {
        return postorder(1, n);
    }
};
```
```

class Solution {
public:
    /**
     * 
     * @param n int整型 
     * @return int整型
     */
    int numTrees(int n) {
        vector<int> dp(n+1, 0);
        dp[0] = 1, dp[1] = 1;
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= i; ++j) {
                dp[i] += dp[j-1] * dp[i-j];
            }
        }
        return dp[n];
    }
};
```

## 二叉树的中序遍历

描述
> 给出一棵二叉树，返回这棵树的中序遍历
> 例如：
> 给出的二叉树为{1,#,2,3}
```
/**
 * struct TreeNode {
 *  int val;
 *  struct TreeNode *left;
 *  struct TreeNode *right;
 * };
 */

class Solution {
public:
    /**
     * 
     * @param root TreeNode类 
     * @return int整型vector
     */
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        if (!root) return res;
        stack<TreeNode *> s;
        TreeNode *cur = root;
        while (!s.empty() || cur) {
            while (cur) {
                s.push(cur);
                cur = cur->left;
            }
            cur = s.top();
            res.push_back(cur->val);
            s.pop();
            cur = cur->right;
        }
        return res;
    }
};
```

## 数字字符串转化成IP地址























