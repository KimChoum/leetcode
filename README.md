# leetcode刷题

## Day 1 : 
### 704 二分查找
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

### [35](https://leetcode.com/problems/search-insert-position/) .搜索插入位置
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

### [34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) 在排序数组中查找元素的第一个和最后一个位置
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

### [27](https://leetcode.com/problems/remove-element/submissions/1182358501/) remove element
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

## Day2

### [977](https://leetcode.com/problems/squares-of-a-sorted-array/description/) 有序数组的平方

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

```c++
vector<int> sortedSquares(vector<int>& nums) {
        vector<int> anw(nums.size());
        int left{0};
        int right = nums.size() - 1;
        for(int i = right; i >= 0; i--){
            int tmp;
            if(abs(nums[left]) < abs(nums[right])){
                tmp = nums[right];
                right--;
            }else{
                tmp = nums[left];
                left++;
            }
            anw[i] = tmp * tmp;
        }
        return anw;
    }
```
使用双指针 O(n)

因为有负数，两边对比后merge起来

### [209](https://leetcode.com/problems/minimum-size-subarray-sum/description/) 长度最小的子数组

```c++
    int minSubArrayLen(int target, vector<int>& nums) {
        int left{0}; int right{0};
        int sum{0};
        int anw{INT_MAX};
        while(right < nums.size()){
            sum += nums[right];
            while(sum >= target){
                anw = min(anw, right - left + 1);
                sum -= nums[left];
                left++;
            }
            right++;
        }

        if(anw == INT_MAX) return 0;
        return anw;
    }
```
滑动窗口    O(n)


###[59](https://leetcode.com/problems/spiral-matrix-ii/description/) 螺旋矩阵II

```C++
vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> anw (n, vector<int>(n, 0));
        int startx{0}; int starty{0};
        int count{1};
        int offset{1};
        for(int loop = 0; loop < n / 2; loop++){
            //左到右
            for(int j = starty; j < n - offset; j++){
                anw[startx][j] = count++;
            }
            //上到下
            for(int i = startx; i < n - offset; i++){
                anw[i][n - offset] = count++;
            }
            //右到左
            for(int j = n - offset; j > starty; j--){
                anw[n - offset][j] = count++;
            }
            //下到上
            for(int i = n - offset; i > startx; i--){
                anw[i][startx] = count++;
            }
            startx++;
            starty++;
            offset++;
        }
        if(n % 2 != 0){
            anw[startx][starty] = count;
        }
        return anw;
    }
```


