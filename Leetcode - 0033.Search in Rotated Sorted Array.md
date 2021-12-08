# 33. Search in Rotated Sorted Array
### There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
Example 3:

Input: nums = [1], target = 0
Output: -1
 

Constraints:

1 <= nums.length <= 5000
-104 <= nums[i] <= 104
All values of nums are unique.
nums is an ascending array that is possibly rotated.
-104 <= target <= 104

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length == 0) {
            return -1;
        }
        int left = 0;
        int right = nums.length - 1;
        while(left <= right) {
            //每次重新赋值mid
            int mid = left + (right - left) / 2;
            //每次二分后判断是否为target
            if(nums[mid] == target) {
                return mid;
            }
            //如果左边值小于中间值，则左半部分为有序，右半部分为乱序
            if(nums[left] <= nums[mid]) {
                //在此排列中判断，如果target就在这个有序数列中，则将它的中间值改为right
                if(nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    //如果target在乱序中，则将中间值改为left，再进行循环去验证
                    left = mid + 1;
                }
            } else {
                //如果左边值大于中间值，则右半部分为有序，左半部分为乱序。两个判断语句同上
                if(nums[mid] < target && target <= nums[nums.length - 1]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```
    
