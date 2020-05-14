# 从0开始的力扣记录！
- [数组 - Array](#数组)
- [链表 - LinkedList](#链表)

```text
                          _/                        _/_/                                     
   _/_/_/      _/_/    _/_/_/_/    _/_/          _/      _/  _/_/    _/_/    _/_/_/  _/_/    
  _/    _/  _/    _/    _/      _/_/_/_/      _/_/_/_/  _/_/      _/    _/  _/    _/    _/   
 _/    _/  _/    _/    _/      _/              _/      _/        _/    _/  _/    _/    _/    
_/    _/    _/_/        _/_/    _/_/_/        _/      _/          _/_/    _/    _/    _/     
                                                                               
                                           
   _/_/_/_/    _/_/    _/  _/_/    _/_/    
      _/    _/_/_/_/  _/_/      _/    _/   
   _/      _/        _/        _/    _/    
_/_/_/_/    _/_/_/  _/          _/_/       
                                           
```

## 数组
数组中时常会用到双指针法，题目中的双指针可能是同向（追及）遍历，也可能是反向（相遇）遍历。根据题目的不同可能会涉及哈希表、二分查找等知识点

- **No.1 - [两数之和](https://leetcode-cn.com/problems/two-sum)**

  利用暴力法时间复杂度比较高，为`O(n^2)`
  题目中另一标签是哈希表，可以采用`unordered_map`（C++）或`HashMap`（Java）来存储已经遍历过且表中没有另一半的元素，时间复杂度可以降到`O(n)`
  
- **No.26 - [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)**

  由于数组已经是排好序的，根据题目要求空间复杂度为`O(1)`，采用双指针法遍历即可

- **No.11 - [盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water)**

  暴力法时间复杂度比较高，为`O(n^2)`，作为中等难度的题目相比AC不了。
  想到长方形的面积是由其宽度和高度决定的，高度一定的情况下越宽越好，然而在宽度缩小的情况下高度越高才能保证面积最大。利用这两个性质采用双指针法。
  双向同时向中间遍历，当遍历完整个数组后即可得到答案
  
- **No.55 - [跳跃游戏](https://leetcode-cn.com/problems/jump-game)**

  由题意判断是否能跳至数组尾端，采用贪心的思想进行解题。
  本人的解题思路是选择能够跳得最远的下一个点进行跳跃，而官方题解的思路在于选择最远的距离进行跳跃，略有不同。
  本人的代码略长但时间却比官方题解要少那么1ms，舒服

- **No.56 - [合并区间](https://leetcode-cn.com/problems/merge-intervals)**

  首先对区间列表根据每个区间的左端进行排序，Java在`Arrays.sort`中使用`new Comparator<T>(){@Override public int compare(T a, T b){}}`。
  对于b区间的左端小于等于a区间的右端的情况，考虑合并a区间与b区间，取a区间的左端以及a、b两区间右端的最大值作为新区间的左右两端。
  
[BACK TO TOP ⬆︎](#从0开始的力扣记录)

## 链表
在专栏中作者推荐了5道需要多写多练的题目：206、141、21、19、876。除了做这些以外，我根据力扣添加其拓展题目

- **No.206 - [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)**

  自己只想到了迭代的解法：每次迭代都建立一个临时结点来存储子节点，在迭代的过程当中注意指针的指向。
  官方给出的递归的方法是假设链表其余部分已经反转，再反转前面部分的链表：
  ```java
  public ListNode reverseList(ListNode head) {
      if (head == null || head.next == null) return head;
      ListNode p = reverseList(head.next);
      head.next.next = head;
      head.next = null;
      return p;
  }
  ```
  
  [BACK TO TOP ⬆︎](#从0开始的力扣记录)
