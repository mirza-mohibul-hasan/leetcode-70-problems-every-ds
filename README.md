# 70 LeetCode Problems | Every Data Structure
## Big O
<img width="1390" height="804" alt="image" src="https://github.com/user-attachments/assets/e29ade7a-e156-43a7-8fd0-02fb5fe7ddd3" />
<img width="1437" height="795" alt="image" src="https://github.com/user-attachments/assets/1e5b8223-ba16-4b49-a3a0-99f2b6cd9921" />
<img width="1442" height="629" alt="image" src="https://github.com/user-attachments/assets/1c6c6801-b76e-46bd-86e6-9ec24cf3979b" />

## Arrays

### 1. Duplicate values
[Contains Duplicate](https://leetcode.com/problems/contains-duplicate/description/)

**Summary:** Check if an array contains any duplicates.

**Approach 1:** Nested Loop
```c++
// Time Complexity: O(n^2)
// Space Complexity: O(1)
bool containsDuplicate(vector<int>& nums) {
    for (int i = 0; i < nums.size(); i++) {
        for (int j = i + 1; j < nums.size(); j++) {
            if (nums[i] == nums[j]) return true;
        }
    }
    return false;
}
```
**Approach-02:** Using Set
```c++
// Time Complexity: O(n)
// Space Complexity: O(n)
bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> st(nums.begin(), nums.end());
    return nums.size() != st.size();
}
```

### 2. Missing Values
[Missing Number](https://leetcode.com/problems/missing-number/description/)  

**Summary:** Check if an array have missing numbner.

**Approach 1:** Sum of first `n` numbers is `n*(n+1)/2`. Subtract the sum of array elements to find the missing number.
```c++
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];
        }
        int total = nums.size() * (nums.size() + 1) / 2;
        return total - sum;
    }
};
```
**Approach-02:** XOR all array elements with all numbers from 0 to n. The result is the missing number.
```c++
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int missing = 0;
        for (int i = 0; i < nums.size(); i++) {
            missing ^= nums[i];
        }
        for (int i = 0; i <= nums.size(); i++) {
            missing ^= i;
        }
        return missing;
    }
};
```

### 3. All Missing Numbers
[Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/)  

**Summary:** Given an array of integers where `1 ≤ nums[i] ≤ n` (n = size of array), some elements appear twice and others are missing. Return all the numbers in the range `[1, n]` that do not appear in the array.
**Approach 1:** Insert all elements into set, then check from 1 to n that which numbners are missing.
```c++
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        unordered_set<int>us(nums.begin(), nums.end());
        int n = nums.size();
        vector<int>ans;
        for(int i = 1; i<=n; i++){
            if(us.find(i) == us.end()){
                ans.emplace_back(i);
            }
        }
        return ans;
    }
};
```
**Approach-02:** In Place Swapping. The array has numbers from `1` to `n`, some may be missing. The key insight is that if every number were in its "ideal" position (i.e., `nums[i] == i+1`), then the ones that are misplaced indicate missing numbers.
```c++
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans;
        for (int i = 0; i < n; i++) {
            if (nums[abs(nums[i]) - 1] > 0) {
                nums[abs(nums[i]) - 1] *= -1;
            }
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) {
                ans.push_back(i + 1);
            }
        }
        return ans;
    }
};
```

### 4. Sum of `nums[i] + nums[j] = target`
[Two sum](https://leetcode.com/problems/two-sum/)  

**Problem:** Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`. You may assume that each input would have exactly one solution, and you may not use the same element twice.

**Approach 1:** Brute Force
```c++
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```
**Approach-02:** Use hashmap, then search.
```c++
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> ans;

        for (int i = 0; i < nums.size(); i++) {
            if (ans.count(nums[i])) {
                return {ans[nums[i]], i};
            } else {
                ans[target - nums[i]] = i;
            }
        }

        return {-1, -1};
    }
};
```

### 5. Smaller Than current number.
[How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number)  

**Problem:** Given the array `nums`, for each `nums[i]` find out how many numbers in the array are smaller than it. That is, for each `nums[i]` you have to count the number of valid `j`'s such that `j != i` and `nums[j] < nums[i]`.

Return the answer in an array.

**Approach 1:** Brute Force. For each element, check all others and count how many are smaller. Straightforward but inefficient.
```c++
// Time Complexity: O(n^2)
// Space Complexity: O(n)
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; i++) {
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                if (nums[j] < nums[i] && j != i) {
                    cnt++;
                }
            }
            ans[i] = cnt;
        }
        return ans;
    }
};
```
**Approach-02:** Use hashmap, then search. Sort the array. For each number, its position in the sorted array equals the count of numbers smaller than it. Use a hashmap to avoid recomputation.
```c++
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int> temp = nums;
        sort(temp.begin(), temp.end());

        unordered_map<int, int> mp;
        mp[temp[0]] = 0;

        //[1, 2, 2, 3, 8]
        //[1->0, 2->1, 2->1, 3->3, 8->4]
        for (int i = 1; i < temp.size(); i++) {
            if (mp.count(temp[i]) == 0) {
                mp[temp[i]] = i;
            }
        }
        vector<int>ans(nums.size());
        for(int i = 0; i<nums.size(); i++){
            ans[i] = mp[nums[i]];
        }
        return ans;
    }
};
```

