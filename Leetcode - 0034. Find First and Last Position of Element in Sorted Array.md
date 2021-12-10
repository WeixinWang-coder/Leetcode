# 34. Find First and Last Position of Element in Sorted Array
###
Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
Example 2:

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
Example 3:

Input: nums = [], target = 0
Output: [-1,-1]
 

Constraints:

0 <= nums.length <= 105
-109 <= nums[i] <= 109
nums is a non-decreasing array.
-109 <= target <= 109

__解题__
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        //base case
        if(nums.length == 0 || nums == null) {
            return new int[] {-1, -1};
        }
        int first = findFrist(nums, target);
        int last = findRight(nums, target);
        return new int[] {first, last};
    }
    
    private int findFrist(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] == target) {
            /*
            如果找到目标值，则需要将right移动至mid位置，因为有可能这个target左边还有目标值；
            但也不可以直接写right = mid - 1,因为有可能左边没有target值，如果减一了就略过了目标值了，
            找右边第一个值思路同此。
            */
                right = mid;
            } else if (nums[mid] > target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        return nums[left] == target ? left : -1;
    }
    
    private int findRight(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while(left < right) {
            int mid = left + (right - left + 1) / 2;
            if(nums[mid] == target) {
                left = mid;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        return nums[left] == target ? left : -1;
    }
}
```

__例子1：__  
target number = 6   
4 5 6 6 7              
L   M   R   
L M R   
    L   
备注：当目标值在其中， 当left = right 时候可以直接 return left   

__例子2：__  
target number = 4   
                    
4    5 6 7
L    m   R
L(m) R
R
备注：当目标值在其中， 当left = right 时候也可以直接 return left

__例子3：__      
target number = 6   
4 5 7    8          left < right return left == target ? left : -1;   
L M      R   
    L(M) R   
    R   
备注：当目标值不在其中， 当left = right 时候可以如果也是return left那得到的不是目标值，所以这里加个判断   

__时间复杂度:__ log(N)  
__空间复杂度:__ O(1)


