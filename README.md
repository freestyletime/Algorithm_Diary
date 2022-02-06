# Algorithm_Diary

## Algorithm Index
1. [Two Sum](https://github.com/freestyletime/Algorithm_Diary#1-two-sum)
2. [Add Two Numbers](https://github.com/freestyletime/Algorithm_Diary#2-add-two-numbers)
3. [Palindrome Number](https://github.com/freestyletime/Algorithm_Diary#3-palindrome-number)
4. [Longest Substring Without Repeating Characters](https://github.com/freestyletime/Algorithm_Diary#4-longest-substring-without-repeating-characters)
5. [Roman to Integer && Integer to Roman](https://github.com/freestyletime/Algorithm_Diary#5-roman-to-integer--integer-to-roman)
6. [Longest Common Prefix](https://github.com/freestyletime/Algorithm_Diary#6-longest-common-prefix)
7. [Search Insert Position](https://github.com/freestyletime/Algorithm_Diary#7-search-insert-position)
8. [Longest Palindromic Substring](https://github.com/freestyletime/Algorithm_Diary#7-longest-palindromic-substring)
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
### Approach 1: Generally Loop two list simultaneously

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

## 4. Longest Substring Without Repeating Characters

Given a string **s**, find the length of the longest substring without repeating characters.

Example 1:

``
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
``

Example 2:

``
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
``

Example 3:

``
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
``

---------------
### Approach 1: Brute Force
Check all the substring one by one to see if it has no duplicate character.
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();

        int res = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if (checkRepetition(s, i, j)) {
                    res = Math.max(res, j - i + 1);
                }
            }
        }

        return res;
    }

    private boolean checkRepetition(String s, int start, int end) {
        int[] chars = new int[128];

        for (int i = start; i <= end; i++) {
            char c = s.charAt(i);
            chars[c]++;
            if (chars[c] > 1) {
                return false;
            }
        }

        return true;
    }
}
```

### Approach 2: Sliding Window
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] chars = new int[128];

        int left = 0;
        int right = 0;

        int res = 0;
        while (right < s.length()) {
            char r = s.charAt(right);
            chars[r]++;

            while (chars[r] > 1) {
                char l = s.charAt(left);
                chars[l]--;
                left++;
            }

            res = Math.max(res, right - left + 1);

            right++;
        }
        return res;
    }
}
```

### Approach 2: Sliding Window Optimized

**Solution 1:**
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```


**Solution 2:**
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int start = 0;
        int max = 0;
        StringBuilder tmp = new StringBuilder();
        for(int i = 0; i < s.length(); i++) {
            String c =  Character.toString(s.charAt(i));
            int end = tmp.lastIndexOf(c);
            if(end != -1) if(end >= start) start = end + 1;
            
            max = Math.max(max, i - start + 1);
            tmp.append(c);
        }
        
        return max;
    }
}
```


**Solution 3(Assuming ASCII 128):**
The previous implements all have no assumption on the charset of the string **s**.

If we know that the charset is rather small, we can replace the Map with an integer array as direct access table.

Commonly used tables are:

- int[26] for Letters 'a' - 'z' or 'A' - 'Z'
- int[128] for ASCII
- int[256] for Extended ASCII
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        Integer[] chars = new Integer[128];

        int left = 0;
        int right = 0;

        int res = 0;
        while (right < s.length()) {
            char r = s.charAt(right);

            Integer index = chars[r];
            if (index != null && index >= left && index < right) {
                left = index + 1;
            }

            res = Math.max(res, right - left + 1);

            chars[r] = right;
            right++;
        }

        return res;
    }
}
```

## 5. Roman to Integer && Integer to Roman

Create a RomanNumerals class that can convert a roman numeral to and from an integer value. It should follow the API demonstrated in the examples below. Multiple roman numeral values will be tested for each helper method.

Modern Roman numerals are written by expressing each digit separately starting with the left most digit and skipping any digit with a value of zero. In Roman numerals 1990 is rendered: 1000=M, 900=CM, 90=XC; resulting in MCMXC. 2008 is written as 2000=MM, 8=VIII; or MMVIII. 1666 uses each Roman symbol in descending order: MDCLXVI.

Example 1:

``
RomanNumerals.toRoman(1000) // should return 'M'
RomanNumerals.fromRoman("M") // should return 1000
``

---------------
### Approach: General Parse

**Solution:**
```java
import java.util.LinkedHashMap;
import java.util.Map.Entry;
import java.util.Map;
public class RomanNumerals {
 
    static Map<String, Integer> nRM = new LinkedHashMap<>();
    static {
          nRM.put("M", 1000); nRM.put("CM", 900); nRM.put("D", 500); nRM.put("CD", 400);
          nRM.put("C", 100); nRM.put("XC", 90); nRM.put("L", 50); nRM.put("XL", 40);
          nRM.put("X", 10); nRM.put("IX", 9); nRM.put("V", 5); nRM.put("IV", 4); 
          nRM.put("I", 1);
    }

    public static String toRoman(int n) {
        StringBuffer sb = new StringBuffer();
        for (Entry<String, Integer> entry : nRM.entrySet()) {
            int base = n / entry.getValue();
            n = n - base * entry.getValue();
            if (base > 0) for (int x = 0; x < base; x++) sb.append(entry.getKey());
            if (n == 0) break;
        }
        return sb.toString();
    }

    public static int fromRoman(String romanNumeral) {
        int sum = 0;
        for (Entry<String, Integer> entry : nRM.entrySet()) {
            String base = entry.getKey();
            int count = 0;
            while(romanNumeral.startsWith(base)){
                romanNumeral = romanNumeral.substring(base.length());
                count += 1;
            }
            sum += count * entry.getValue();
            if(romanNumeral.isEmpty()) break;
        }
        return sum;
    }
}
```

## 6. Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string **""**.

Example 1:

``
Input: strs = ["flower","flow","flight"]
Output: "fl"
``

Example 2:

``
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
``

---------------
### Approach 1: Horizontal scanning
Suppose the first element in the array is the common prefix for all elements. Loop the array while cutting the common prefix till the end to check if the common prefix is valid for each element.
```java
public String longestCommonPrefix(String[] strs) {
    String common = strs[0];
    for (String str : strs) {
        while(!str.	startsWith(common)){
            if(common.isEmpty()) return common;
            common = common.substring(0, common.length() - 1);
        }
    }
    return common;
}
```


### Approach 2: Vertical scanning
Imagine a very short string is the common prefix at the end of the array. The above approach will still do S comparisons. One way to optimize this case is to do vertical scanning. We compare characters from top to bottom on the same column (same character index of the strings) before moving on to the next column.
```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    for (int i = 0; i < strs[0].length() ; i++){
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j ++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c)
                return strs[0].substring(0, i);             
        }
    }
    return strs[0];
}
```

## 7. Search Insert Position
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with **O(log n)** runtime complexity.

Example 1:

``
Input: nums = [1,3,5,6], target = 5
Output: 2
``

Example 2:

``
Input: nums = [1,3,5,6], target = 2
Output: 1
``

Example 3:

``
Input: nums = [1,3,5,6], target = 7
Output: 4
``

---------------
### Approach: Binary Search

**solution 1:**
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        if (nums.length == 0) return -1;
        int left = 0;
        int right = nums.length;

        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid;
            }
        }
        return left;
    }
}
```

**solution 2:**
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        return searchInsert(nums, target, 0, nums.length - 1);
    }
    
    public int searchInsert(int[] nums, int target, int start, int end) {
        if(target <= nums[start]) return start;
        else if(target == nums[end]) return end;
        else if(target > nums[end]) return end + 1;
        else if(start == end){
            if(target > nums[start]) return start + 1; else return start;
        }else{
            int mid = (end + start + 1) / 2;
            if(target == nums[mid]) return mid;
            else if(target > nums[mid]) return searchInsert(nums, target, mid + 1, end);
            else return searchInsert(nums, target, start, mid - 1);
        }
    }
}
```

## 8. Longest Palindromic Substring
Given a string **s**, return the longest palindromic substring in **s**.

Example 1:

``
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
``

Example 2:

``
Input: s = "cbbd"
Output: "bb"
``

---------------
### Approach 1: Brute Force
Some people will be tempted to come up with a quick solution, which is unfortunately flawed (however can be corrected easily):

```java
class Solution {
    
    public boolean isPalindrome(String s){
        for(int i = 0; i< s.length() / 2; i ++) 
            if(s.charAt(i) != s.charAt(s.length() - i - 1)) 
                return false;
        return true;
    }
    
    public String longestPalindrome(String s) {
        for(int i = 0; i < s.length(); i++) {
            int step = s.length() - i;
            int start= 0;
            int end  = step;
            while(end <= s.length()){
                String x = s.substring(start++, end++);
                if(isPalindrome(x)) return x;
            }
        }
        return "";
    }
}
```

### Approach 2: Longest Common Substring
Reverse S and become S'
Find the longest common substring between S and S', which must also be the longest palindromic substring.

This seemed to work, let’s see some examples below.

For example, S = "caba", S'= "abac".
The longest common substring between S and S' is "aba", which is the answer.


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