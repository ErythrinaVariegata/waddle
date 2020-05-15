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
  
  ```java
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return null;
    }
  ```
  
- **No.26 - [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)**

  由于数组已经是排好序的，根据题目要求空间复杂度为`O(1)`，采用双指针法遍历即可
  
  ```java
    public int removeDuplicates(int[] nums) {
      int pos = 0;
      int next = 0;
      while (next < nums.length) {
        while (next < nums.length && nums[next] == nums[pos]) {
          next++;
        }
        
        if (next == pos + 1) {
          pos++;
        } else if (next >= nums.length) {
          break;
        } else {
          nums[++pos] = nums[next++];
        }
      }
      
      return pos + 1;
    }
  ```

- **No.11 - [盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water)**

  暴力法时间复杂度比较高，为`O(n^2)`，作为中等难度的题目相比AC不了。
  想到长方形的面积是由其宽度和高度决定的，高度一定的情况下越宽越好，然而在宽度缩小的情况下高度越高才能保证面积最大。利用这两个性质采用双指针法。
  双向同时向中间遍历，当遍历完整个数组后即可得到答案
  
  ```java
    public int maxArea(int[] height) {
      int left = 0;
      int right = height.length - 1;
      int area = 0;
      while (left < right) {
        area = Math.max(Math.min(height[left], height[right]) * (right - left), area);
        if (height[left] < height[right]) {
          left++;
        } else {
          right--;
        }
      }
      return area;
    }
  ```
  
- **No.55 - [跳跃游戏](https://leetcode-cn.com/problems/jump-game)**

  由题意判断是否能跳至数组尾端，采用贪心的思想进行解题。
  本人的解题思路是选择能够跳得最远的下一个点进行跳跃，而官方题解的思路在于选择最远的距离进行跳跃，略有不同。
  本人的代码略长但时间却比官方题解要少那么1ms，舒服
  
  ```java
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int rightmost = 0;
        for (int i = 0; i < n; ++i) {
            if (i <= rightmost) {
                rightmost = Math.max(rightmost, i + nums[i]);
                if (rightmost >= n - 1) {
                    return true;
                }
            }
        }
        return false;
    }
  ```

- **No.56 - [合并区间](https://leetcode-cn.com/problems/merge-intervals)**

  首先对区间列表根据每个区间的左端进行排序，
  对于b区间的左端小于等于a区间的右端的情况，考虑合并a区间与b区间，取a区间的左端以及a、b两区间右端的最大值作为新区间的左右两端。
  
  ```java
    public int[][] merge(int[][] intervals) {
      if (intervals == null || intervals.length <= 1) {
        return intervals;
      }
      Arrays.sort(intervals, new Comparator<int[]>(){
        @Override
        public int compare(int[] a, int[] b) {
          return a[0] - b[0];
        }
      });
      ArrayList<int[]> list = new ArrayList<>();
      int i=0;
      int n = intervals.length;
      while(i<n){
          int left = intervals[i][0];
          int right = intervals[i][1];
          while(i < n - 1 && right >= intervals[i+1][0]){
              right = Math.max(right, intervals[i+1][1]);
              i++;
          }
          list.add(new int[] {left,right});
          i++;
      }
      return list.toArray(new int[list.size()][2]);
    }
  ```
  
[BACK TO TOP ⬆︎](#从0开始的力扣记录)

## 链表
在专栏中作者推荐了5道需要多写多练的题目：206、141、21、19、876。除了做这些以外，我根据力扣添加其拓展题目

- **No.206 - [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)**

  自己只想到了迭代的解法：每次迭代都建立一个临时结点来存储子节点，在迭代的过程当中注意指针的指向。
  官方给出的递归的方法是假设链表其余部分已经反转，再反转前面部分的链表。

  ```java
   // Solution1: 迭代
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
  // Solution2: 递归
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode p = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return p;
    }
  ```
  
- **No.92 - [反转链表II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)**

  这一题是上一题的拓展，利用4个指针很快能够得到答案，画图能够很好的帮助理解反转的过程。
  
  ```java
    public ListNode reverseBetween(ListNode head, int m, int n) {
      ListNode prev = null;
      ListNode curr = head;
      for (int i = 1; i < m; i++) {
          prev = curr;
          curr = curr.next;
      }
      ListNode prevPar = prev;
      ListNode tail = curr;
      ListNode currCld = null;
      for (int i = 0; i <= n - m; i++) {
          currCld = curr.next;
          curr.next = prev;
          prev = curr;
          curr = currCld;
      }
      if (prevPar == null) {
          head = prev;
      } else {
          prevPar.next = prev;
      }
      tail.next = currCld;
      return head; 
    }
  ```

- **No.141 - [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)**

  快慢指针追及，要是能够追上说明有环路存在，如果存在快指针跑到终点，很明显链表中无环。

  ```java
   public boolean hasCycle(ListNode head) {
      ListNode fast = head;
      ListNode slow = head;
      while (fast != null && fast.next != null) {
          if (fast == slow) {
              return slow;
          }
          fast = fast.next.next;
          slow = slow.next;
      }
      return null;
  }
  ```

- **No.142 - [环形链表II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)**

  利用上一个问题的结果找到环的相交点，采用弗洛伊德法获得环的起始点。
  
  ```java
    public ListNode getIntersect(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            if (fast == slow) {
                return slow;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        return null;
    }

    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        // 检查链表中是否有环存在
        ListNode intersect = getIntersect(head);
        if (intersect == null) {
            return null;
        }
        // 找到相交点，慢指针继续移动，快指针转变成慢指针向原慢指针移动
        ListNode ptr1 = head;
        ListNode ptr2 = intersect;
        while (ptr1 != ptr2) {
            ptr1 = ptr1.next;
            ptr2 = ptr2.next;
        }
        return ptr1;
    }
  ```

- **No.21 - [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)**

  采用归并排序的办法解决。快得很！

  ```java
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
      ListNode head = new ListNode(-1);
      ListNode dummyHead = head;
      while (l1 != null && l2 != null) {
          if (l1.val <= l2.val) {
              dummyHead.next = l1;
              l1 = l1.next;
          } else {
              dummyHead.next = l2;
              l2 = l2.next;
          }
          dummyHead = dummyHead.next;
      }
      while (l1 != null) {
          dummyHead.next = l1;
          l1 = l1.next;
          dummyHead = dummyHead.next;
      }
      while (l2 != null) {
          dummyHead.next = l2;
          l2 = l2.next;
          dummyHead = dummyHead.next;
      }
      return head.next;
    }
  ```

- **No.19 - [删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)**

  采用双指针法解决。
  
  ```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
      ListNode sentry = head;
      for (int i = 0; i < n; i++) {
          sentry = sentry.next;
      }
      if (sentry == null) {
          return head.next;
      }
      
      ListNode prev = head;
      while (sentry.next != null) {
          prev = prev.next;
          sentry = sentry.next;
      }
      prev.next = prev.next.next;
      return head;
    }
  ```
  
- **No.876 - [链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)**

  采用双指针法解决。
  
  ```java
    public ListNode middleNode(ListNode head) {
      ListNode tail = head;
      ListNode mid = head;
      while (tail != null) {
        if (tail.next == null) {
          break;
        }
        tail = tail.next.next;
        mid = mid.next;
      }
      if (tail != null && tail.next != null) {
          mid = mid.next;
      }
      return mid;
    }
  ```

[BACK TO TOP ⬆︎](#从0开始的力扣记录)
