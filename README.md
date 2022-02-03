# Algorithm_Diary

## Algorithm Index
1. Two Sum
2. Add Two Numbers
3. Palindrome Number
---------------
## 1. Two Sum

Given an array of integers **nums** and an integer **target**, return indices of the two numbers such that they add up to **target**.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

Example 1:

``
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
``

Example 2:

``
Input: nums = [3,2,4], target = 6
Output: [1,2]
``

Example 3:

``
Input: nums = [3,3], target = 6
Output: [0,1]
``

---------------
### Approach 1: Brute Force

The brute force approach is simple. Loop through each element xx and find if there is another value that equals to **target - x**.
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == target - nums[i]) {
                    return new int[] { i, j };
                }
            }
        }
        // In case there is no solution, we'll just return null
        return null;
    }
}
```

### Approach 2: One-pass Hash Table 

It turns out we can do it in one-pass. While we are iterating and inserting elements into the hash table, we also look back to check if current element's complement already exists in the hash table. If it exists, we have found a solution and return the indices immediately.
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        // In case there is no solution, we'll just return null
        return null;
    }
}
```

## 2. Add Two Numbers

You are given two **non-empty** linked lists representing two **non-negative** integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example 1:

``
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
``

Example 2:

``
Input: l1 = [0], l2 = [0]
Output: [0]
``

Example 3:

``
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
``

---------------
### Approach 1: General Loop two list simultaneously

The pseudocode is as following:
- Initialize a interim node to represent the ListNode we will make.
- Initialize current node to dummy head of the returning list.
- Initialize a boolean variable **flag** to control whether add 1 or not in the next loop.
- Loop through lists **l1** and **l2** until you reach both ends and check if it is necessary to add one node to the tail of the list that we will return.
- = = = Loop start = = =
- Set **pre** to the intact next node value.
- Add the l1's node value to **pre** if l1 hasn't been reached the end and then advance l1.
- Add the l2's node value to **pre** if l2 hasn't been reached the end and then advance l2.
- If **flag** is true it indicates **pre** is greater than 10 in the last loop, so we need do **pre** plus 1.
- Initialize **val** that is the final value of next node and give the remainder of **pre** to it.
- Check if **pre** is greater than 10 **flag** will be true otherwise **flag** will be false.
- Initialize next node and let its value equals to **val**
- Let the interim node point to the next node.
- = = = Loop end = = =


```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode tmp = new ListNode();
        ListNode head = tmp;
        boolean flag = false;
        while(l1 != null || l2 != null || flag){
            int pre = 0;
            if(l1 != null) {
                pre += l1.val;
                l1 = l1.next;
            }
            if(l2 != null) {
                pre += l2.val;
                l2 = l2.next;
            }
            if(flag == true)  pre += 1;
            
            int val = (int) pre % 10;
            flag = (pre / 10) > 0 ? true : false;
            
            tmp.next = new ListNode(val);
            tmp = tmp.next;
        }
        return head.next;
    }
}
```

### Approach 2: Recursion

Same as approach 1.
```java
public ListNode resList = new ListNode(0);
public ListNode head = resList;
public int carry = 0;

public ListNode addTwoNumbers(ListNode l1, ListNode l2) { 
    int sum = l1.val + l2.val + carry;
    carry  = sum / 10;
    resList.next = new ListNode(sum % 10);
    resList = resList.next;

    if(l1.next != null && l2.next != null)
        addTwoNumbers(l1.next, l2.next);  
    else if (l1.next != null)
        addTwoNumbers(l1.next, new ListNode(0)); 
    else if (l2.next != null)
        addTwoNumbers(new ListNode(0), l2.next);   
    else if (carry > 0)
    {
        resList.next = new ListNode(1);
        resList = resList.next;
    }     
    return head.next;
}
```

## 3. Palindrome Number

Given an integer **x**, return true if **x** is palindrome integer.

An integer is a **palindrome** when it reads the same backward as forward.

- For example, 121 is a palindrome while 123 is not.

Example 1:

``
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
``

Example 2:

``
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
``

Example 3:

``
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
``

Constraints:

``
-2^31 <= x <= 2^31 - 1
``

---------------
### Approach: Revert half of the number

The first idea that comes to mind is to convert the number into string, and check if the string is a palindrome, but this would **require extra non-constant space** for creating the string which is not allowed by the problem description.

Second idea would be reverting the number itself, and then compare the number with original number, if they are the same, then the number is a palindrome. However, if the reversed number is larger than int.MAX, we will hit integer overflow problem.

Following the thoughts based on the second idea, to avoid the overflow issue of the reverted number, what if we only revert half of the int number? After all, the reverse of the last half of the palindrome should be the same as the first half of the number, if the number is a palindrome.
```java
public bool IsPalindrome(int x) {
        // Special cases:
        // As discussed above, when x < 0, x is not a palindrome.
        // Also if the last digit of the number is 0, in order to be a palindrome,
        // the first digit of the number also needs to be 0.
        // Only 0 satisfy this property.
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while(x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // When the length is an odd number, we can get rid of the middle digit by revertedNumber/10
        // For example when the input is 12321, at the end of the while loop we get x = 12, revertedNumber = 123,
        // since the middle digit doesn't matter in palidrome(it will always equal to itself), we can simply get rid of it.
        return x == revertedNumber || x == revertedNumber/10;
}
```

# LICENSE 
MIT License

Copyright (c) 2022 Chris Chen

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.