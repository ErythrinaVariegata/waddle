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
- [数组 - Array](#数组)

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
