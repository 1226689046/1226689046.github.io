---
layout: post
title: leetcode0002
tags:数据结构和算法
- 数据结构和算法
categories: 数据结构和算法
description: 算法学习
---
# leetcode0002

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

##### 思路

很显然，将其拼接成整数，然后使用字符串反转，就可以。。。但是没那么简单啊。。

##### 代码

```java
StringBuilder s1 = new StringBuilder();
StringBuilder s2 = new StringBuilder();
while (l1 != null) {
    s1.append(l1.val);
    l1 = l1.next;
}
while (l2 != null) {
    s2.append(l2.val);
    l2 = l2.next;
}
s1.reverse();
s2.reverse();
String[] r = (Integer.valueOf(s1.toString()) + Integer.valueOf(s2.toString()) + "").split("");
ListNode result = new ListNode(Integer.parseInt(r[r.length-1]));
ListNode t = result;
for (int i = r.length-2; i >= 0; i--) {
    ListNode l = new ListNode(Integer.parseInt(r[i]));
    t.next = l;
    t = l;
}
return result;
```

改为long也不可以，因为他的数字可以非常非常大

决定使用加法，使用数组存储起来，然后逐位进行判断

##### 第二次提交

```java
public ListNode addTwoNumbers2(ListNode l1, ListNode l2) {
    int thisJY = 0;
    ListNode h = new ListNode(0);
    ListNode r = h;
    while (l1 != null && l2 != null) {
        int i = l1.val;
        int j = l2.val;
        int s = i + j + thisJY;
        thisJY = s / 10;
        r.val = s % 10;
        if (l1.next != null){
            r.next = new ListNode(0);
            r = r.next;}
        l1 = l1.next;
        l2 = l2.next;
    }
    if (thisJY == 1) {
        r.next = new ListNode(1);
    }
    //最后再判断一下进位
    return h;
}
```

这种情况并没有判断那啥。。两者长度不一样的情况，再从后面进行判断就可以了。

```java
public ListNode addTwoNumbers2(ListNode l1, ListNode l2) {
    int thisJY = 0;
    ListNode h = new ListNode(0);
    ListNode r = h;
    ListNode pre = null;
    while (l1 != null || l2 != null) {
        int i = l1 == null ? 0 : l1.val;
        int j = l2 == null ? 0 : l2.val;

        int s = i + j + thisJY;
        thisJY = s / 10;

        r.val = s % 10;
        pre = r;
        r.next = new ListNode(0);
        r = r.next;
        l1 = l1 == null ? null : l1.next;
        l2 = l2 == null ? null : l2.next;
    }
    if (thisJY == 1) {
        pre.next = new ListNode(1);
    } else {
        pre.next = null;
    }
    //最后再判断一下进位
    return h;
}
```

运行结果还算可以。主要是要考虑到两个链表的长度可能不相同。

每一次循环都记录下进位和当前位的数字