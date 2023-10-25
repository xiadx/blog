---
title: 剑指Offer
date: 2023-09-12 16:26:00
tags: Algorithm
categories: Algorithm
---

[牛客网剑指Offer编程题](https://www.nowcoder.com/exam/oj/ta?page=1&tpId=13&type=13)

## 二维数组中的查找
题目描述

> 在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```
# -*- coding:utf-8 -*-
class Solution:
    # array 二维列表
    def Find(self, target, array):
        # write code here
        if not array:
            return False
        else:
            rows, cols = len(array), len(array[0])
            r, c = 0, cols - 1
            while r <= rows - 1 and c >= 0:
                if array[r][c] == target:
                    return True
                elif array[r][c] < target:
                    r += 1
                else:
                    c -= 1
            else:
                return False
```

## 替换空格
题目描述

> 请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
```
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        r = ''
        for c in s:
            if c != ' ':
                r += c
            else:
                r += '%20'
        return r
```

## 从尾到头打印链表
题目描述

> 输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        h = listNode
        r = []
        while h:
            r.insert(0, h.val)
            h = h.next
        return r
```

## 重建二叉树
题目描述

> 输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        if not pre:
            return None
        else:
            value = pre[0]
            root = TreeNode(value)
            try:
                rindex = tin.index(value)
            except:
                return None
            left = self.reConstructBinaryTree(pre[1:rindex+1], tin[:rindex])
            right = self.reConstructBinaryTree(pre[rindex+1:], tin[rindex+1:])
            root.left, root.right = left, right
            return root
```

## 用两个栈实现队列
题目描述

> 用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
```
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.stack_first = []
        self.stack_second = []
    def push(self, node):
        # write code here
        self.stack_first.append(node)
    def pop(self):
        # return xx
        if not self.stack_first and not self.stack_second:
            return None
        elif self.stack_second:
            return self.stack_second.pop()
        else:
            while self.stack_first:
                self.stack_second.append(self.stack_first.pop())
            return self.stack_second.pop()
```

## 旋转数组的最小数字
题目描述

> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
```
# -*- coding:utf-8 -*-
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        # write code here
        if not rotateArray:
            return 0
        else:
            left, right = 0, len(rotateArray) - 1
            value = rotateArray[-1]
            while left <= right:
                mid = (left + right) / 2
                if rotateArray[mid] > value:
                    left = mid + 1
                else:
                    right = mid - 1
            return rotateArray[left]
```

## 斐波那契数列
题目描述

> 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39
```
# -*- coding:utf-8 -*-
class Solution:
    def Fibonacci(self, n):
        # write code here
        x, y = 0, 1
        for _ in range(n):
            x, y = y, x + y
        return x
```

## 跳台阶
题目描述

>一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。
```
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloor(self, number):
        # write code here
        x, y = 1, 1
        for _ in range(number):
            x, y = y, x + y
        return x
```

## 变态跳台阶
题目描述

>  一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
```
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloorII(self, number):
        # write code here
        return 1 << (number - 1)
```

## 矩形覆盖
题目描述

> 我们可以用21的小矩形横着或者竖着去覆盖更大的矩形。请问用n个21的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？
```
# -*- coding:utf-8 -*-
class Solution:
    def rectCover(self, number):
        # write code here
        if number == 0:
            return 0
        else:
            x, y = 1, 1
            for _ in range(number):
                x, y = y, x + y
            return x
```

## 二进制中1的个数
题目描述

> 输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
```
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1(self, n):
        # write code here
        return sum([(n>>i & 1) for i in range(32)])
```

## 数值的整数次方
题目描述

> 给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。
```
# -*- coding:utf-8 -*-
class Solution:
    def Power(self, base, exponent):
        # write code here
        if exponent == 0:
            return 1
        elif exponent > 0:
            if exponent & 1:
                return base * self.Power(base, exponent/2) ** 2
            else:
                return self.Power(base, exponent/2) ** 2
        else:
            return 1.0 / self.Power(base, -exponent)
```

## 调整数组顺序使奇数位于偶数前面
题目描述

> 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。
```
# -*- coding:utf-8 -*-
class Solution:
    def reOrderArray(self, array):
        # write code here
        odd, even = [], []
        for a in array:
            if a & 1:
                odd.append(a)
            else:
                even.append(a)
        return odd + even
```

## 链表中倒数第k个结点
题目描述

> 输入一个链表，输出该链表中倒数第k个结点。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def FindKthToTail(self, head, k):
        # write code here
        if k <= 0:
            return None
        else:
            p = head
            q = head
            while k and q:
                q = q.next
                k -= 1
            if k > 0:
                return None
            else:
                while q:
                    p = p.next
                    q = q.next
                return p
```

## 反转链表
题目描述

> 输入一个链表，反转链表后，输出新链表的表头。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        # write code here
        if not pHead:
            return None
        else:
            q, r = pHead, pHead.next
            q.next = None
            while r:
                p = q
                q = r
                r = r.next
                q.next = p
            return q
```

## 合并两个排序的链表
题目描述

> 输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        # write code here
        if not pHead1 and not pHead2:
            return None
        else:
            if not pHead1:
                return pHead2
            elif not pHead2:
                return pHead1
            else:
                if pHead1.val <= pHead2.val:
                    head = pHead1
                    head.next = self.Merge(pHead1.next, pHead2)
                else:
                    head = pHead2
                    head.next = self.Merge(pHead1, pHead2.next)
                return head
```

## 树的子结构
题目描述

> 输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def dfs(self, r1, r2):
        if not r2:
            return True
        elif not r1:
            return False
        else:
            if r1.val != r2.val:
                return False
            else:
                return self.dfs(r1.left, r2.left) and self.dfs(r1.right, r2.right)
    
    def HasSubtree(self, pRoot1, pRoot2):
        # write code here
        if not pRoot1:
            return False
        elif not pRoot2:
            return False
        else:
            return self.dfs(pRoot1, pRoot2) or self.HasSubtree(pRoot1.left, pRoot2) or self.HasSubtree(pRoot1.right, pRoot2)
```

## 二叉树的镜像
题目描述

> 操作给定的二叉树，将其变换为源二叉树的镜像。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回镜像树的根节点
    def Mirror(self, root):
        # write code here
        if not root:
            return None
        else:
            left = self.Mirror(root.left)
            right = self.Mirror(root.right)
            root.left, root.right = right, left
            return root
```

## 顺时针打印矩阵
题目描述

> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.
```
# -*- coding:utf-8 -*-
class Solution:
    # matrix类型为二维列表，需要返回列表
    def printMatrix(self, matrix):
        # write code here
        if not matrix:
            return []
        else:
            res = []
            rows, cols = len(matrix), len(matrix[0])
            l, r, t, b = 0, cols - 1, 0, rows - 1
            while l <= r and t <= b:
                for i in range(l, r+1):
                    res.append(matrix[t][i])
                if t < b:
                    for i in range(t+1, b+1):
                        res.append(matrix[i][r])
                    if r > l:
                        for i in range(r-1, l-1, -1):
                            res.append(matrix[b][i])
                        if t < b - 1:
                            for i in range(b-1, t, -1):
                                res.append(matrix[i][l])
                            l += 1
                            r -= 1
                            t += 1
                            b -= 1
                        else:
                            break
                    else:
                        break
                else:
                    break
            return res
```

## 包含min函数的栈
题目描述

> 定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。
```
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.stack_first = []
        self.stack_second = []
    def push(self, node):
        # write code here
        self.stack_first.append(node)
        if not self.stack_second:
            self.stack_second.append(node)
        else:
            value = self.stack_second[-1]
            if node <= value:
                self.stack_second.append(node)
            else:
                self.stack_second.append(value)
    def pop(self):
        # write code here
        self.stack_first.pop()
        self.stack_second.pop()
    def top(self):
        # write code here
        return self.stack_first[-1]
    def min(self):
        # write code here
        return self.stack_second[-1]
```

## 栈的压入、弹出序列
题目描述

> 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）
```
# -*- coding:utf-8 -*-
class Solution:
    def IsPopOrder(self, pushV, popV):
        # write code here
        n = len(pushV)
        if n == 0:
            return False
        else:
            res = []
            j = 0
            for i in range(n):
                res.append(pushV[i])
                while j < n and res[-1] == popV[j]:
                    res.pop()
                    j += 1
            if not res:
                return True
            else:
                return False
```

## 从上往下打印二叉树
题目描述

> 从上往下打印出二叉树的每个节点，同层节点从左至右打印。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回从上到下每个节点值列表，例：[1,2,3]
    def PrintFromTopToBottom(self, root):
        # write code here
        if not root:
            return []
        else:
            res = []
            q = [root]
            while q:
                v = q.pop(0)
                res.append(v.val)
                if v.left:
                    q.append(v.left)
                if v.right:
                    q.append(v.right)
            return res
```

## 二叉搜索树的后序遍历序列
题目描述

> 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。
```
# -*- coding:utf-8 -*-
class Solution:
    def VerifySquenceOfBST(self, sequence):
        # write code here
        if not sequence:
            return False
        else:
            value = sequence[-1]
            n = len(sequence)
            i = 0
            while i < n and sequence[i] < value:
                i += 1
            while i < n and sequence[i] > value:
                i += 1
            if i == n - 1:
                return True
            else:
                return False
```

## 二叉树中和为某一值的路径
题目描述

> 输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回二维列表，内部每个列表表示找到的路径
    def FindPath(self, root, expectNumber):
        # write code here
        if not root:
            return []
        else:
            if root.val == expectNumber and not root.left and not root.right:
                return [[root.val]]
            else:
                left = self.FindPath(root.left, expectNumber - root.val)
                right = self.FindPath(root.right, expectNumber - root.val)
                return [[root.val] + l for l in left if l] + [[root.val] + r for r in right if r]
```

## 复杂链表的复制
题目描述

> 输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）
```
# -*- coding:utf-8 -*-
# class RandomListNode:
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None
class Solution:
    # 返回 RandomListNode
    def Clone(self, pHead):
        # write code here
        if not pHead:
            return None
        else:
            p = pHead
            while p:
                q = RandomListNode(p.label)
                q.next = p.next
                p.next = q
                p = q.next
                
            p = pHead
            while p:
                q = p.next
                if p.random:
                    q.random = p.random.next
                else:
                    pass
                p = q.next
                
            p = pHead
            h = p.next
            while p.next:
                q = p.next
                p.next = q.next
                p = q
            return h
```

## 二叉搜索树与双向链表
题目描述

> 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Convert(self, pRootOfTree):
        root = pRootOfTree
        if not root:
            return None
        else:
            self.Convert(root.left)
            left = root.left
            if left:
                while left.right:
                    left = left.right
                root.left, left.right = left, root
                
            self.Convert(root.right)
            right = root.right
            if right:
                while right.left:
                    right = right.left
                root.right, right.left = right, root
                
            while root.left:
                root = root.left
                
            return root
```

## 字符串的排列
题目描述

> 输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。
```
# -*- coding:utf-8 -*-
import itertools
class Solution:
    def Permutation(self, ss):
        # write code here
        if not ss:
            return []
        else:
            return sorted(list(set(map(''.join, itertools.permutations(ss)))))
```

## 数组中出现次数超过一半的数字
题目描述

> 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。
```
import collections
# -*- coding:utf-8 -*-
class Solution:
    def MoreThanHalfNum_Solution(self, numbers):
        # write code here
        counter = collections.Counter(numbers)
        for k, v in counter.items():
            if v > len(numbers)/2:
                return k
        return 0
```

## 最小的K个数
题目描述

> 输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。
```
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        if k <= 0 or k > len(tinput):
            return []
        else:
            tinput.sort()
            return tinput[:k]
```

## 连续子数组的最大和
题目描述

> HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)
```
# -*- coding:utf-8 -*-
class Solution:
    def FindGreatestSumOfSubArray(self, array):
        # write code here
        n = len(array)
        if n == 0:
            return 0
        else:
            dp = [0] * n
            dp[0] = array[0]
            for i in range(1, n):
                dp[i] = max(dp[i-1]+array[i], array[i])
            return max(dp)
```

## 整数中1出现的次数（从1到n整数中1出现的次数）
题目描述

> 求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。
```
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1Between1AndN_Solution(self, n):
        # write code here
        def nOf1(q):
            index = 0
            for i in str(q):
                if i == '1':
                    index += 1
            return index
        sum = 0
        for q in range(1, n + 1):
            sum += nOf1(q)
        return sum
```

## 把数组排成最小的数
题目描述

> 输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。
```
# -*- coding:utf-8 -*-
class Solution:
    def PrintMinNumber(self, numbers):
        # write code here
        def compare(x, y):
            a = int(str(x) + str(y))
            b = int(str(y) + str(x))
            if a > b:
                return 1
            elif a < b:
                return -1
            else:
                return 0
            
        numbers.sort(cmp=compare)
        res = ''
        for n in numbers:
            res += str(n)
        return res
```

## 丑数
题目描述

> 把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。
```
# -*- coding:utf-8 -*-
class Solution:
    def GetUglyNumber_Solution(self, index):
        # write code here
        if index < 1:
            return 0
        UglyNum = [1]
        indexTwo = 0
        indexThree = 0
        indexFive = 0
        for i in range(index - 1):
            NewUgly = min(UglyNum[indexTwo] * 2, UglyNum[indexThree] * 3, UglyNum[indexFive] * 5)
            UglyNum.append(NewUgly)
            if NewUgly % 2 == 0:
                indexTwo += 1
            if NewUgly % 3 == 0:
                indexThree += 1
            if NewUgly % 5 == 0:
                indexFive += 1
        return UglyNum[-1]
```

## 第一个只出现一次的字符位置
题目描述

> 在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.
```
# -*- coding:utf-8 -*-
import collections
class Solution:
    def FirstNotRepeatingChar(self, s):
        # write code here
        if not s:
            return -1
        counter = collections.Counter(s)
        for i, c in enumerate(s):
            if counter[c] == 1:
                return i
```

## 数组中的逆序对
题目描述

> 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007
```
# -*- coding:utf-8 -*-
class Solution:
    def InversePairsCore(self, data, copy, start, end):
        if start == end:
            return 0
        else:
            mid = (start + end) / 2
            left = self.InversePairsCore(copy, data, start, mid)
            right = self.InversePairsCore(copy, data, mid+1, end)
            i, j = mid, end
            count = 0
            copyIndex = end
            while i >= start and j >= mid + 1:
                if data[i] > data[j]:
                    count += j - mid
                    copy[copyIndex] = data[i]
                    copyIndex -= 1
                    i -= 1
                else:
                    copy[copyIndex] = data[j]
                    copyIndex -= 1
                    j -= 1
            while i >= start:
                copy[copyIndex] = data[i]
                copyIndex -= 1
                i -= 1
            while j >= mid + 1:
                copy[copyIndex] = data[j]
                copyIndex -= 1
                j -= 1
            return left + right + count
                    
    def InversePairs(self, data):
        # write code here
        return self.InversePairsCore(data[:], data[:], 0, len(data) - 1) % 1000000007
```

## 两个链表的第一个公共结点
题目描述

> 输入两个链表，找出它们的第一个公共结点。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def FindFirstCommonNode(self, pHead1, pHead2):
        # write code here
        s1 = []
        s2 = []
        p = pHead1
        q = pHead2
        while p:
            s1.append(p)
            p = p.next
        while q:
            s2.append(q)
            q = q.next
            
        c = None
        while s1 and s2:
            if s1[-1] == s2[-1]:
                c = s1.pop()
                s2.pop()
            else:
                break
        return c
```

## 数字在排序数组中出现的次数
题目描述

> 统计一个数字在排序数组中出现的次数。
```
# -*- coding:utf-8 -*-
class Solution:
    def GetNumberOfK(self, data, k):
        # write code here
        left, right = 0, len(data) - 1
        while left <= right:
            mid = (left + right) / 2
            if data[mid] < k:
                left = mid + 1
            else:
                right = mid - 1
        i = left
        left, right = 0, len(data) - 1
        while left <= right:
            mid = (left + right) / 2
            if data[mid] <= k:
                left = mid + 1
            else:
                right = mid - 1
        j = left
        return j - i
```

## 二叉树的深度
题目描述

> 输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def TreeDepth(self, pRoot):
        # write code here
        if not pRoot:
            return 0
        else:
            return max(self.TreeDepth(pRoot.left), self.TreeDepth(pRoot.right)) + 1
```

## 平衡二叉树
题目描述

> 输入一棵二叉树，判断该二叉树是否是平衡二叉树。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def depth(self, pRoot):
        if not pRoot:
            return 0
        else:
            return max(self.depth(pRoot.left), self.depth(pRoot.right)) + 1
    def IsBalanced_Solution(self, pRoot):
        # write code here
        if not pRoot:
            return True
        else:
            left = self.depth(pRoot.left)
            right = self.depth(pRoot.right)
            if left == right or abs(left - right) == 1:
                return self.IsBalanced_Solution(pRoot.left) and self.IsBalanced_Solution(pRoot.right)
            else:
                return False
```

## 数组中只出现一次的数字
题目描述

> 一个整型数组里除了两个数字之外，其他的数字都出现了偶数次。请写程序找出这两个只出现一次的数字。
```
# -*- coding:utf-8 -*-
import collections
class Solution:
    # 返回[a,b] 其中ab是出现一次的两个数字
    def FindNumsAppearOnce(self, array):
        # write code here
        counter = collections.Counter(array)
        res = []
        for a in array:
            if counter[a] == 1:
                res.append(a)
        return res
```

## 和为S的连续正数序列
题目描述

> 小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!
```
import math
# -*- coding:utf-8 -*-
class Solution:
    def FindContinuousSequence(self, tsum):
        # write code here
        res = []
        i, j = 2, int(math.sqrt(2*tsum))
        for n in range(j, i-1, -1):
            if n & 1 and tsum % n == 0:
                r = []
                m = tsum / n
                for x in range(m - (n-1)/2, m + (n-1)/2 + 1):
                    r.append(x)
                res.append(r)
            elif n & 1 == 0 and (tsum % n) * 2 == n:
                r = []
                m = ((2 * tsum) / n) / 2
                for x in range(m-n/2+1, m+n/2 + 1):
                    r.append(x)
                res.append(r)
        return res
```

## 和为S的两个数字
题目描述

> 输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。
```
# -*- coding:utf-8 -*-
class Solution:
    def FindNumbersWithSum(self, array, tsum):
        # write code here
        d = {}
        for i, a in enumerate(array):
            d[a] = i
        r = []
        m = float('inf')
        for v in d:
            if (tsum-v) in d:
                if v * (tsum-v) < m:
                    m = v * (tsum-v)
                    r = [v, tsum-v]
        return r
```

## 左旋转字符串
题目描述

> 汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！
```
# -*- coding:utf-8 -*-
class Solution:
    def LeftRotateString(self, s, n):
        # write code here
        if n <= 0:
            return s
        if not s:
            return ""
        n = n % len(s)
        a = s[:n][::-1]
        b = s[n:][::-1]
        return (a + b)[::-1]
```

## 翻转单词顺序列
题目描述

> 牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？
```
# -*- coding:utf-8 -*-
class Solution:
    def ReverseSentence(self, s):
        # write code here
        r = ""
        i = 0
        start = 0
        count = 0
        while i < len(s):
            while i < len(s) and s[i] is not ' ':
                count += 1
                i += 1
            r += s[start:start + count][::-1]
            while i < len(s) and s[i] is ' ':
                r += ' '
                i += 1
            start = i
            count = 0
        return r[::-1]
```

## 扑克牌顺子
题目描述

> LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)…他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子…..LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。
```
# -*- coding:utf-8 -*-
class Solution:
    def IsContinuous(self, numbers):
        # write code here
        if not numbers:
            return False
        else:
            numbers.sort()
            n = len(numbers)
            i = 0
            zeros = 0
            while i < n:
                if numbers[i] == 0:
                    zeros += 1
                i += 1
            if zeros == n - 1:
                return True
            else:
                i = zeros
                while i < n - 1:
                    gap = numbers[i + 1] - numbers[i] - 1
                    if gap > zeros or gap < 0:
                        return False
                    else:
                        zeros -= gap
                    i += 1
                return True
```

## 孩子们的游戏(圆圈中最后剩下的数)
题目描述

> 每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0…m-1报数….这样下去….直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)
```
# -*- coding:utf-8 -*-
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None
class Solution:
    def LastRemaining_Solution(self, n, m):
        # write code here
        if n == 0:
            return -1
        h = ListNode(0)
        p = h
        for i in range(1, n):
            p.next = ListNode(i)
            p = p.next
        p.next = h
        while h != h.next:
            for i in range(1, m):
                p = p.next
                h = h.next
            h = h.next
            p.next = h
        return h.val
```

## 求1+2+3+…+n
题目描述

> 求1+2+3+…+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。
```
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.r = 0
    def Sum_Solution(self, n):
        # write code here
        def recursive(n):
            self.r += n
            n -= 1
            return (n>0) and recursive(n)
        recursive(n)
        return self.r
```

## 不用加减乘除做加法
题目描述

> 写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。
```
# -*- coding:utf-8 -*-
class Solution:
    def Add(self, num1, num2):
        # write code here
        return sum([num1, num2])
```

## 把字符串转换成整数
题目描述

> 将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。
```
# -*- coding:utf-8 -*-
class Solution:
    def StrToInt(self, s):
        # write code here
        if not s:
            return 0
        if not s[0].isdigit() and s[0] is not '+' and s[0] is not '-':
            return 0
        if s[0] is '+' and len(s) == 1 or s[0] is '-' and len(s) == 1:
            return 0
        for i in range(1, len(s)):
            if not s[i].isdigit():
                return 0
        if s[0] == '-':
            return -int(s[1:])
        elif s[0] == '+':
            return int(s[1:])
        else:
            return int(s)
```

## 数组中重复的数字
题目描述

> 在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。
```
# -*- coding:utf-8 -*-
import collections
class Solution:
    # 这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    # 函数返回True/False
    def duplicate(self, numbers, duplication):
        # write code here
        if not numbers:
            return False
        else:
            counter = collections.Counter(numbers)
            for n in numbers:
                if counter[n] != 1:
                    duplication[0] = n
                    return True
            return False
```

## 构建乘积数组
题目描述

> 给定一个数组A[0,1,…,n-1],请构建一个数组B[0,1,…,n-1],其中B中的元素B[i]=A[0]A[1]…A[i-1]A[i+1]…A[n-1]。不能使用除法。
```
# -*- coding:utf-8 -*-
class Solution:
    def multiply(self, A):
        # write code here
        if not A:
            return []
        else:
            n = len(A)
            B = [0] * n
            B[0] = 1
            for i in range(1, n):
                B[i] = B[i - 1] * A[i - 1]
            temp = 1
            for j in range(n - 2, -1, -1):
                temp *= A[j + 1]
                B[j] *= temp
            return B
```

## 正则表达式匹配
题目描述

> 请实现一个函数用来匹配包括’.’和’‘的正则表达式。模式中的字符’.’表示任意一个字符，而’‘表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串”aaa”与模式”a.a”和”abaca”匹配，但是与”aa.a”和”ab*a”均不匹配
```
class Solution(object):
    def match(self, text, pattern):
        if not pattern:
            return not text
        first_match = bool(text) and pattern[0] in {text[0], '.'}
        if len(pattern) >= 2 and pattern[1] == '*':
            return (self.match(text, pattern[2:]) or
                    first_match and self.match(text[1:], pattern))
        else:
            return first_match and self.match(text[1:], pattern[1:])
```

## 表示数值的字符串
题目描述

> 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串”+100”,”5e2”,”-123”,”3.1416”和”-1E-16”都表示数值。 但是”12e”,”1a3.14”,”1.2.3”,”+-5”和”12e+4.3”都不是。
```
# -*- coding:utf-8 -*-
class Solution:
    # s字符串
    def isNumeric(self, s):
        # write code here
        try:
            p = float(s)
            return True
        except:
            return False
```

## 字符流中第一个不重复的字符
题目描述

> 请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符”go”时，第一个只出现一次的字符是”g”。当从该字符流中读出前六个字符“google”时，第一个只出现一次的字符是”l”。
```
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.buffer = ''
    # 返回对应char
    def FirstAppearingOnce(self):
        # write code here
        appear = [False for _ in range(256)]
        for b in self.buffer:
            if appear[ord(b)] is False:
                appear[ord(b)] = True
            else:
                appear[ord(b)] = False
        for b in self.buffer:
            if appear[ord(b)] is True:
                return b
        return '#'

    def Insert(self, char):
        # write code here
        self.buffer += char
```

## 链表中环的入口结点
题目描述

> 给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def EntryNodeOfLoop(self, pHead):
        # write code here
        if not pHead or not pHead.next or not pHead.next.next:
            return None
        else:
            slow = pHead.next
            fast = pHead.next.next
            while slow != fast:
                if fast and fast.next:
                    slow = slow.next
                    fast = fast.next.next
                else:
                    return None
            slow = pHead
            while slow != fast:
                slow = slow.next
                fast = fast.next
            return slow
```

## 删除链表中重复的结点
题目描述

> 在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def deleteDuplication(self, pHead):
        # write code here
        p, q, r = None, pHead, None
        while q:
            if q.next and q.next.val == q.val:
                r = q.next
                while r.next and r.next.val == q.val:
                    r = r.next
                if q == pHead:
                    pHead = r.next
                else:
                    p.next = r.next
                q = r.next
            else:
                p = q
                q = q.next
        return pHead
```

## 二叉树的下一个结点
题目描述

> 给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。
```
# -*- coding:utf-8 -*-
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None
class Solution:
    def GetNext(self, pNode):
        # write code here
        if not pNode:
            return None
        else:
            if pNode.right:
                res = pNode.right
                while res.left:
                    res = res.left
                return res
            else:
                parent = pNode.next
                current = pNode
                while parent and parent.left != current:
                    current = parent
                    parent = parent.next
                return parent
```

## 对称的二叉树
题目描述

> 请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def mirror(self, pRoot):
        if not pRoot:
            return None
        else:
            left = self.mirror(pRoot.left)
            right = self.mirror(pRoot.right)
            pRoot.left, pRoot.right = right, left
            return pRoot
        
    def sample(self, r1, r2):
        if not r1 and not r2:
            return True
        else:
            return (r1 and r2) and (r1.val == r2.val) and self.sample(r1.left, r2.left) and self.sample(r1.right, r2.right)
    
    def isSymmetrical(self, pRoot):
        # write code here
        if not pRoot:
            return True
        else:
            left, right = pRoot.left, pRoot.right
            right = self.mirror(right)
            return self.sample(left, right)
```

## 按之字形顺序打印二叉树
题目描述

> 请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Print(self, pRoot):
        # write code here
        if not pRoot:
            return []
        else:
            res = []
            s1, s2 = [pRoot], []
            while s1 or s2:
                if s1:
                    r = []
                    while s1:
                        v = s1.pop()
                        r.append(v.val)
                        if v.left:
                            s2.append(v.left)
                        if v.right:
                            s2.append(v.right)
                    res.append(r)
                if s2:
                    r = []
                    while s2:
                        v = s2.pop()
                        r.append(v.val)
                        if v.right:
                            s1.append(v.right)
                        if v.left:
                            s1.append(v.left)
                    res.append(r)
            return res
```

## 把二叉树打印成多行
题目描述

> 从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回二维列表[[1,2],[4,5]]
    def Print(self, pRoot):
        # write code here
        if not pRoot:
            return []
        else:
            res = []
            q1, q2 = [pRoot], []
            while q1 or q2:
                if q1:
                    r = []
                    while q1:
                        v = q1.pop(0)
                        r.append(v.val)
                        if v.left:
                            q2.append(v.left)
                        if v.right:
                            q2.append(v.right)
                    res.append(r)
                if q2:
                    r = []
                    while q2:
                        v = q2.pop(0)
                        r.append(v.val)
                        if v.left:
                            q1.append(v.left)
                        if v.right:
                            q1.append(v.right)
                    res.append(r)
            return res
```

## 序列化二叉树
题目描述

> 请实现两个函数，分别用来序列化和反序列化二叉树
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    flag = -1
    
    def Serialize(self, root):
        # write code here
        if not root:
            return '#'
        else:
            return str(root.val) + ',' + self.Serialize(root.left) + ',' + self.Serialize(root.right)

    def Deserialize(self, s):
        # write code here
        self.flag += 1
        if self.flag >= len(s):
            return None
        root = None
        l = s.split(',')
        if l[self.flag] != '#':
            root = TreeNode(int(l[self.flag]))
            root.left = self.Deserialize(s)
            root.right = self.Deserialize(s)
        return root
```

## 二叉搜索树的第k个结点
题目描述

> 给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8） 中，按结点数值大小顺序第三小结点的值为4。
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    count = 0
    # 返回对应节点TreeNode
    def KthNode(self, pRoot, k):
        # write code here
        if not pRoot or k <= 0:
            return None
        else:
            res = self.KthNode(pRoot.left, k)
            if res:
                return res
            self.count += 1
            if self.count == k:
                return pRoot
            res = self.KthNode(pRoot.right, k)
            if res:
                return res
            return None
```

## 数据流中的中位数
题目描述

> 如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。
```
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.data=[]
    def Insert(self, num):
        # write code here
        self.data.append(num)
        self.data.sort()
    def GetMedian(self, data):
        # write code here
        n = len(self.data)
        if n % 2 == 0:
            return (self.data[n/2] + self.data[n/2-1]) / 2.0
        else:
            return self.data[n/2]
```

## 滑动窗口的最大值
题目描述

> 给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。
```
# -*- coding:utf-8 -*-
class Solution:
    def maxInWindows(self, num, size):
        # write code here
        if size <= 0:
            return []
        res = []
        for i in xrange(0, len(num)-size+1):
            res.append(max(num[i:i+size]))
        return res
```

## 矩阵中的路径
题目描述

> 请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则之后不能再次进入这个格子。 例如 a b c e s f c s a d e e 这样的3 X 4 矩阵中包含一条字符串”bcced”的路径，但是矩阵中不包含”abcb”路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。
```
# -*- coding:utf-8 -*-
class Solution:
    def hasPath(self, matrix, rows, cols, path):
        # write code here
        for i in range(rows):
            for j in range(cols):
                if matrix[i*cols+j] == path[0]:
                    if self.find(list(matrix), rows, cols, path[1:], i, j):
                        return True
        return False
    def find(self, matrix, rows, cols, path, i, j):
        if not path:
            return True
        matrix[i*cols+j] = '0'
        if j+1 < cols and matrix[i*cols+j+1] == path[0]:
            return self.find(matrix, rows, cols, path[1:], i, j+1)
        elif j-1 >= 0 and matrix[i*cols+j-1] == path[0]:
            return self.find(matrix, rows, cols, path[1:], i, j-1)
        elif i+1 < rows and matrix[(i+1)*cols+j] == path[0]:
            return self.find(matrix, rows, cols, path[1:], i+1, j)
        elif i-1 >= 0 and matrix[(i-1)*cols+j] == path[0]:
            return self.find(matrix, rows, cols, path[1:], i-1, j)
        else:
            return False
```

## 机器人的运动范围
题目描述

> 地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？
```
# -*- coding:utf-8 -*-
class Solution:
    result = 0
    
    def movingCount(self, threshold, rows, cols):
        # write code here
        record = [[True] * cols for _ in range(rows)]
        
        def compute_index_sum(number):
            return sum(map(int, [_ for _ in str(number)]))
        
        def is_valid(i, j):
            if 0 <= i < rows and 0 <= j < cols and record[i][j] and compute_index_sum(i) + compute_index_sum(j) <= threshold:
                return True
            else:
                return False
            
        def dfs(i, j):
            if is_valid(i, j):
                record[i][j] = False
                self.result += 1
                dfs(i - 1, j)
                dfs(i + 1, j)
                dfs(i, j - 1)
                dfs(i, j + 1)
                
        dfs(0, 0)
        
        return self.result
```