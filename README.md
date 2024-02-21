# leetcode刷题

## Day 1 : 
  704 二分查找
  ```c++

      int search(vector<int>& nums, int target) {
        int left{0}; int right = nums.size() - 1;
        while(left <= right){
            int mid = (right - left) / 2 + left; // 可防止溢出
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return -1;
    };
```
使用左闭右闭

[35](https://leetcode.com/problems/search-insert-position/) .搜索插入位置
```c++
    int searchInsert(vector<int>& nums, int target) {
        int left{0}; int right = nums.size() - 1;
        while(left <= right){
            int mid = (right - left) / 2 + left;
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return left;
    }
```
使用左闭右闭， 找不到位置的时候left总会在应该插入的位置

[34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) 在排序数组中查找元素的第一个和最后一个位置
```c++
vector<int> searchRange(vector<int>& nums, int target) {
        return {bf(nums, target, 0), bf(nums, target, 1)};
    }

    int bf(vector<int>& nums, int target, int flag){ // flag == 0 search left, otherwise, search right
        int left{0}; int right = nums.size() - 1;
        while(left <= right){
            int mid = (right - left) / 2 + left;
            if(nums[mid] == target){
                if(!flag){
                    if(mid == 0 || nums[mid - 1] != target){
                        return mid;
                    }else{
                        right = mid;
                    }
                }else{
                    if(mid == nums.size() - 1|| nums[mid + 1] != target){
                        return mid;
                    }else{
                        left = mid + 1; // 注意！！ [2,2]情况 在寻找右边
                    }   
                }
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return -1;
    }
```

[27](https://leetcode.com/problems/remove-element/submissions/1182358501/) remove element
```c++
int removeElement(vector<int>& nums, int val) {
        int left{0};
        int right = nums.size() - 1;
        while(left <= right){
            if(nums[left] == val){
                nums[left] = nums[right];
                right--;
            }else{
                left++;
            }
        }
        return right + 1;
    }
```
如果需要完整的保留元素顺序，可使用快慢针（不是上面的代码）
如果没有强制要求可使用双指针一左一右


