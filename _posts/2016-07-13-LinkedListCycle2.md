---
layout: post
title: 142. Linked List Cycle II
categories: [Leetcode]
tags: [Leetcode]
fullview: true
---

### 问题描述 ###
Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

**Note:** Do not modify the linked list.

**Follow up:** Can you solve it without using extra space?

给定一串链表，找到链表中环的开始位置，若不存在环，返回`null`。

**Note:** 不要修改链表。

**Follow up:** 尝试不使用额外的空间解决问题。

### 解决方案 ###

**分析：**定义两个指针`slow`和`fast`同时从链表`head`出发，`slow`一次前进一个节点，`fast`一次前进两个节点。若 `fast`或`slow`移动到了链表尾，即 `fast == null || slow == null`，说明该链表不存在环；若`slow`和 `fast`相遇，则说明链表中存在环。

**步骤：**

1. 找到`slow`和`fast`相遇的点。记链表头到环之间的距离为`A`，环的长度为`L`，环起始点到相遇点的距离为`B`，相遇点到环起点的距离为`C = L - B`，相遇时`slow`绕环选择了`n1`圈，相遇时`fast`绕环选择了`n2`圈，则有：`2 * (A + B + n1 * L) == A + B + n2 * L`；
2. 对步骤1公式进行处理得：`A = (n2 - 2 * n1) * L - B`，将`C = L - B`代入得`A = (n2 - 2 * n1 - 1) * L + C`等价于`A % L == C`，即再两个指针同时从链表头和步骤1中的相遇点出发，这两个指针将会在环起始点相遇。

**思考**

- 为何要将`C = L - B`代入？什么情况下可以不用代入？

**代码片段**

	public ListNode detectCycle(ListNode head) {
      if (head == null || head.next == null || head.next.next == null) {
        return null;
      }
      ListNode slow = head.next;
      ListNode fast = head.next.next;
      //Step 1: 找到slow和fast相遇的点
      while (slow != fast) {
        if (fast.next == null || fast.next.next == null) {
          return null;
        }
        slow = slow.next;
        fast = fast.next.next;
      }
      //Step 2: 让指针从起点和相遇点同时出发
      slow = head;
      while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
      }
      return slow;
	}
