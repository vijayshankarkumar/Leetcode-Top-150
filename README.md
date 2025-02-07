# Leetcode Top 150

#### 1. 88. Merge Sorted Array

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

Example 1:

Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

Example 2:

Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].

Example 3:

Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        // Keep the merged result the empty spaces in nums1 if that is
        // filled then start filling from 0th index in nums2
        for (int i = 0, j = 0, k = m; i < m || j < n;) {
            k = k == m + n ? 0 : k;
            if (i == m) nums1[k++] = nums2[j++];
            else if (j == n) nums1[k++] = nums1[i++];
            else if (nums1[i] <= nums2[j]) nums1[k++] = nums1[i++];
            else nums1[k++] = nums2[j++];
        }
        // By now nums1 will be form, for ex: [3, 4, 5, 1, 2] then shift this
        // array n times
        reverse(nums1.begin(), nums1.end());
        reverse(nums1.begin(), nums1.begin() + n);
        reverse(nums1.begin() + n, nums1.end());
    }
};
```

#### 2. 27. Remove Element
    
Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.
Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

Example 1:

Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        // [i ... j] will contain all the values equal to val
        int i = -1;
        for (int j = 0; j < nums.size(); j++) {
            if (nums[j] != val) {
                swap(nums[i + 1], nums[j]);
                i++;
            }
        }
        return i + 1;
    }
};
```


#### 3. 26. Remove Duplicates from Sorted Array

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.
Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

Example 1:

Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        // [i ... j] contains all the duplicate values
        int i = 0;
        for (int j = 1; j < nums.size(); j++) {
            if (nums[j] != nums[i]) {
                swap(nums[i + 1], nums[j]);
                i++;
            }
        }
        return i + 1;
    }
};
```

#### 4. 80. Remove Duplicates from Sorted Array II

Given an integer array nums sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

Example 1:

Input: nums = [1,1,1,2,2,3]
Output: 5, nums = [1,1,2,2,3,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,0,1,1,1,1,2,3,3]
Output: 7, nums = [0,0,1,1,2,3,3,_,_]
Explanation: Your function should return k = 7, with the first seven elements of nums being 0, 0, 1, 1, 2, 3 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() <= 2) return nums.size();
        // [i ... j] contains all the values with
        // frequency less than or equal to 2
        int i = 1;
        for (int j = 2; j < nums.size(); j++) {
            if (nums[j] == nums[i] && nums[j] != nums[i - 1]) {
                swap(nums[i + 1], nums[j]);
                i++;
            }
            else if (nums[j] != nums[i]) {
                swap(nums[i + 1], nums[j]);
                i++;
            }
        }
        nums.resize(i + 1);
        return i + 1;  
    }
};
```

#### 5. 169. Majority Element

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

 

Example 1:

Input: nums = [3,2,3]
Output: 3
Example 2:

Input: nums = [2,2,1,1,1,2,2]
Output: 2

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        // boyer moore algorithm
        int me = INT_MIN, cnt = 0;
        for (const auto& num: nums) {
            cnt += num == me ? 1 : -1;
            if (cnt <= 0) {
                me = num;
                cnt = 1;
            }
        }
        return me;
    }
};
```

#### 6. 189. Rotate Array

Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.

 
Example 1:

Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
Example 2:

Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        reverse(nums.begin(), nums.end());
        reverse(nums.begin(), nums.begin() + k % nums.size());
        reverse(nums.begin() + k % nums.size(), nums.end());
    }
};
```

#### 7. 121. Best Time to Buy and Sell Stock

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.


Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
Example 2:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        // take minimum price found so far to calcuate 
        // current profit
        int res = 0, mn = prices.front();
        for (const auto& price: prices) {
            res = max(res, price < mn ? 0 : price - mn);
            mn = min(mn, price);
        }
        return res;
    }
};
```

#### 8. 122. Best Time to Buy and Sell Stock II

You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.


Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
Example 2:

Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
Example 3:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        for(int i = 1 ;i<prices.size() ;i++){
            if(prices[i-1] < prices[i])
                profit += prices[i] - prices[i-1];
        }
        return profit;
    }
};
```

#### 9. 55. Jump Game

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

 
Example 1:

Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
Example 2:

Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int curr = 0, max_so_far = 0;
        for (int i = 0; i < nums.size(); i++) {
            max_so_far = max(max_so_far, i + nums[i]);
            if (i == curr) {
                curr = max_so_far;
                max_so_far = 0;
            }
            if (i > curr) return false;
        }
        return true;
    }
};
```

#### 10. 45. Jump Game II

You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

0 <= j <= nums[i] and
i + j < n
Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

 
Example 1:

Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
Example 2:

Input: nums = [2,3,0,1,4]
Output: 2

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() == 1) return 0;
        int res = 0, curr = 0, max_so_far = 0;
        for (int i = 0; i < nums.size(); i++) {
            max_so_far = max(max_so_far, i + nums[i]);
            if (i == curr) {
                curr = max_so_far;
                max_so_far = 0;
                if (curr >= nums.size() - 1) return res + 1;
                res++;
            }
        }
        return res;
    }
};
```

#### 11. 274. H-Index

Given an array of integers citations where citations[i] is the number of citations a researcher received for their ith paper, return the researcher's h-index.

According to the definition of h-index on Wikipedia: The h-index is defined as the maximum value of h such that the given researcher has published at least h papers that have each been cited at least h times.


Example 1:

Input: citations = [3,0,6,1,5]
Output: 3
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.
Example 2:

Input: citations = [1,3,1]
Output: 1

```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        sort(citations.begin(), citations.end());
        for (int i = 0; i < citations.size(); i++) {
            if (citations[i] >= citations.size() - i) return citations.size() - i;
        }
        return 0;
    }
};
```

#### 12. 238. Product of Array Except Self

Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.


Example 1:

Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Example 2:

Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> res(nums.size());
        for (int i = nums.size() - 1; i >= 0; i--) {
            res[i] = nums[i] * (i == nums.size() - 1 ? 1 : res[i + 1]);
        }
        for (int i = 0, l = 1; i < nums.size(); i++) {
            res[i] = l * (i == nums.size() - 1 ? 1 : res[i + 1]);
            l *= nums[i];
        }
        return res;
    }
};
```

#### 13. 380. Insert Delete GetRandom O(1)

Implement the RandomizedSet class:

RandomizedSet() Initializes the RandomizedSet object.
bool insert(int val) Inserts an item val into the set if not present. Returns true if the item was not present, false otherwise.
bool remove(int val) Removes an item val from the set if present. Returns true if the item was present, false otherwise.
int getRandom() Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the same probability of being returned.
You must implement the functions of the class such that each function works in average O(1) time complexity.
 

Example 1:

Input
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
Output
[null, true, false, true, 2, true, false, 2]

Explanation
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.

```cpp
class RandomizedSet {
private:
    unordered_map<int, int> mp;
    vector<int> st;

public:
    RandomizedSet() {
        
    }
    
    bool insert(int val) {
        if (mp.find(val) != mp.end()) return false;
        mp[val] = st.size();
        st.push_back(val);
        return true;
    }
    
    bool remove(int val) {
        // swap the last element with the element 
        // to be deleted and delete the last element
        if (mp.find(val) != mp.end()) {
            mp[st.back()] = mp[val];
            st[mp[val]] = st.back();
            mp.erase(mp.find(val));
            st.pop_back();
            return true;
        }
        return false;
    }
    
    int getRandom() {
        return st[rand() % st.size()];
    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

#### 14. 134. Gas Station

There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique.


Example 1:

Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
Example 2:

Input: gas = [2,3,4], cost = [3,4,3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        int total_surplus = 0; // it will give us a difference b/w gas & cost
        int surplus = 0;       // our tank
        int start = 0;         // and the index of gas station

        for (int i = 0; i < n; i++) {
            total_surplus += gas[i] - cost[i];
            surplus += gas[i] - cost[i];
            if (surplus < 0) { // if the tank goes -ve
                surplus = 0;   // reset our tank
                start = i + 1; // and update the stating gas station
            }
        }
        return (total_surplus < 0) ? -1 : start;
    }
};
```

#### 15. 135. Candy

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
Return the minimum number of candies you need to have to distribute the candies to the children.

 
Example 1:

Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
Example 2:

Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> cnd(ratings.size());
        vector<pair<int, int>> ri;
        for (int i = 0; i < ratings.size(); i++) ri.push_back({ratings[i], i});
        sort(ri.begin(), ri.end());
        // Iterate through ratings with smaller ratings first as if its neighbours
        // is less than the current, it should be more than their candy
        for (const auto& [rating, idx]: ri) {
            if (idx - 1 >= 0 && ratings[idx - 1] < ratings[idx]) {
                cnd[idx] = cnd[idx - 1] + 1;
            }
            if (idx + 1 < ratings.size() && ratings[idx + 1] < ratings[idx]) {
                cnd[idx] = max(cnd[idx], cnd[idx + 1] + 1);
            }
            if (cnd[idx] == 0) {
                cnd[idx] = 1;
            }
        }
        return accumulate(cnd.begin(), cnd.end(), 0L);
    }
};
```

#### 16. 42. Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
 

Example 1:


Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
Example 2:

Input: height = [4,2,0,3,2,5]
Output: 9

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        vector<int> leftMax(n), rightMax(n);
        for (int i = 0; i < n; i++) {
            leftMax[i] = i == 0 ? height[i] : max(leftMax[i - 1], height[i]);
        }
        for (int i = n - 1; i >= 0; i--) {
            rightMax[i] = i == n - 1 ? height[i] : max(rightMax[i + 1], height[i]);
        }
        int ans = 0;
        for (int i = 1; i < n - 1; i++) {
            int currMax = min(leftMax[i - 1], rightMax[i + 1]);
            if (currMax >= height[i]) ans += currMax - height[i];
        }
        return ans;
    }
};
```

#### 17. 13. Roman to Integer

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

 

Example 1:

Input: s = "III"
Output: 3
Explanation: III = 3.
Example 2:

Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
Example 3:

Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

```cpp
class Solution {
public:
    int char2num(char a) {
        switch (a) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return 0;
        }
    }

    int romanToInt(string s) {
        int result = 0;
        for (int i = 0; i < s.length(); i++) {
            if (i + 1 < s.length() && char2num(s[i]) < char2num(s[i + 1])) {
                result -= char2num(s[i]);
            } else {
                result += char2num(s[i]);
            }
        }
        return result;
    }
};
```

#### 18. 12. Integer to Roman

Seven different symbols represent Roman numerals with the following values:

Symbol	Value
I	1
V	5
X	10
L	50
C	100
D	500
M	1000
Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:

If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.
If the value starts with 4 or 9 use the subtractive form representing one symbol subtracted from the following symbol, for example, 4 is 1 (I) less than 5 (V): IV and 9 is 1 (I) less than 10 (X): IX. Only the following subtractive forms are used: 4 (IV), 9 (IX), 40 (XL), 90 (XC), 400 (CD) and 900 (CM).
Only powers of 10 (I, X, C, M) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (V), 50 (L), or 500 (D) multiple times. If you need to append a symbol 4 times use the subtractive form.
Given an integer, convert it to a Roman numeral.

 

Example 1:

Input: num = 3749

Output: "MMMDCCXLIX"

Explanation:

3000 = MMM as 1000 (M) + 1000 (M) + 1000 (M)
 700 = DCC as 500 (D) + 100 (C) + 100 (C)
  40 = XL as 10 (X) less of 50 (L)
   9 = IX as 1 (I) less of 10 (X)
Note: 49 is not 1 (I) less of 50 (L) because the conversion is based on decimal places
Example 2:

Input: num = 58

Output: "LVIII"

Explanation:

50 = L
 8 = VIII
Example 3:

Input: num = 1994

Output: "MCMXCIV"

Explanation:

1000 = M
 900 = CM
  90 = XC
   4 = IV

```cpp
class Solution {
public:
    string intToRoman(int num) {
        string ones[] = {"","I","II","III","IV","V","VI","VII","VIII","IX"};
        string tens[] = {"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"};
        string hrns[] = {"","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"};
        string ths[]={"","M","MM","MMM"};
        
        return ths[num/1000] + hrns[(num%1000)/100] + tens[(num%100)/10] + ones[num%10];
    }
};
```

#### 19. 58. Length of Last Word

Given a string s consisting of words and spaces, return the length of the last word in the string.

A word is a maximal 
substring
 consisting of non-space characters only.


Example 1:

Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.
Example 2:

Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.
Example 3:

Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.

```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
       istringstream is(s);
       string word;
       int res = 0;
       while (is >> word) res = word.size();
       return res; 
    }
};
```

#### 20. 14. Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".


Example 1:

Input: strs = ["flower","flow","flight"]
Output: "fl"
Example 2:

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
 
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string res;
        while (true) {
            if (res.size() == strs.front().size()) return res;
            char ch = strs.front()[res.size()];
            for (const auto& str: strs) {
                if (str.size() < res.size()) return res;
                if (str[res.size()] != ch) return res;
            }
            res.push_back(ch);
        }
        return "";
    }
};
```

#### 21. 151. Reverse Words in a String

Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

Example 1:

Input: s = "the sky is blue"
Output: "blue is sky the"
Example 2:

Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
Example 3:

Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.

```cpp
class Solution {
public:
    string reverseWords(string s) {
        // make the string evenly spaced with one space between two words
        for (int i = 0, j = 0; j < s.size(); j++) {
            if (s[j] != ' ') {
                if (i == j) i++;
                else {
                    s[i++] = s[j];
                    s[j] = ' '; 
                }
            }
            else {
                if (i == 0 || s[i - 1] == ' ') continue;
                else s[i++] = ' ';
            }
        }
        // erase the remaining spaces at the end
        s.erase(find_if(s.rbegin(), s.rend(), [](unsigned char ch) {
            return !isspace(ch);
        }).base(), s.end());
        // reverse the whole string
        reverse(s.begin(), s.end());
        // reverse the individual words
        for (auto prev = s.begin(), it = s.begin(); it != s.end(); it++) {
            if (*it == ' ') {
                reverse(prev, it);
                prev = it + 1;
            }
            if (it + 1 == s.end()) reverse(prev, s.end());
        }
        return s;
    }
};
```

#### 22. 6. Zigzag Conversion

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);
 

Example 1:

Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
Example 2:

Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
Example 3:

Input: s = "A", numRows = 1
Output: "A"

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows <= 1) return s;
        vector<string> mat(numRows, "");
        int j = 0, dir = -1;
        for (int i = 0; i < s.length(); i++) {
            if (j == numRows - 1 || j == 0) dir *= (-1);
            mat[j] += s[i];
            if (dir == 1) j++;
            else j--;
        }
        string res;
        for (auto& it : mat) res += it;
        return res;
    }
};
```


#### 23. 28. Find the Index of the First Occurrence in a String

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
 

Example 1:

Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
Example 2:

Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        return haystack.find(needle);
    }
};
```

#### 24. 68. Text Justification
Hard
Topics
Companies
Given an array of strings words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly maxWidth characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

Note:

A word is defined as a character sequence consisting of non-space characters only.
Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
The input array words contains at least one word.
 

Example 1:

Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
Example 2:

Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.
Example 3:

Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]

```cpp
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        vector<string> result;
        int n = words.size();
        int index = 0;

        while (index < n) {
            int totalChars = words[index].size();
            int last = index + 1;

            while (last < n && totalChars + words[last].size() + (last - index) <= maxWidth) {
                totalChars += words[last].size();
                last++;
            }

            string line;
            int gaps = last - index - 1;

            if (last == n || gaps == 0) {
                for (int i = index; i < last; i++) {
                    line += words[i];
                    if (i < last - 1) line += " ";
                }
                line += string(maxWidth - line.size(), ' ');
            } else {
                int spaces = (maxWidth - totalChars) / gaps;
                int extraSpaces = (maxWidth - totalChars) % gaps;

                for (int i = index; i < last - 1; i++) {
                    line += words[i];
                    line += string(spaces + (i - index < extraSpaces ? 1 : 0), ' ');
                }
                line += words[last - 1];
            }

            result.push_back(line);
            index = last;
        }

        return result;
    }
};
```

#### 25. 125. Valid Palindrome

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

Example 1:

Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
Example 2:

Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
Example 3:

Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.

```cpp
class Solution {
public:
    bool isAlpha(const char& ch) {
        return !((ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') || (ch >= '0' && ch <= '9'));
    }

    bool isPalindrome(string s) {
        for (int i = 0, j = s.size() - 1; i < j;) {
            if (!isAlpha(s[i]) && !isAlpha(s[j])) {
                if (tolower(s[i]) != tolower(s[j])) return false;
                i++;
                j--;
            }
            else if (isAlpha(s[i])) i++;
            else if (isAlpha(s[j])) j--;
        }
        return true;
    }
};
```

#### 26. 392. Is Subsequence

Given two strings s and t, return true if s is a subsequence of t, or false otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).
 

Example 1:

Input: s = "abc", t = "ahbgdc"
Output: true
Example 2:

Input: s = "axc", t = "ahbgdc"
Output: false

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if (s.empty()) return true;
        for (int i = 0, j = 0; i < s.size() && j < t.size(); j++) {
            if (s[i] == t[j]) i++;
            if (i == s.size()) return true;
        }
        return false;
    }
};
```

#### 27. 167. Two Sum II - Input Array Is Sorted

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.

Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

Example 1:

Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
Example 2:

Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
Example 3:

Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for (int i = 0, j = numbers.size() - 1; i < j;) {
            if (numbers[i] + numbers[j] == target) return {i + 1, j + 1};
            else if (numbers[i] + numbers[j] > target) j--;
            else i++;
        }
        return {-1, -1};
    }
};
```


#### 28. 11. Container With Most Water

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.
 

Example 1:

Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
Example 2:

Input: height = [1,1]
Output: 1

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int res = 0;
        for (int i = 0, j = height.size() - 1; i < j;) {
            res = max(res, min(height[i], height[j]) * (j - i));
            if (height[i] < height[j]) i++;
            else j--;
        }
        return res;
    }
};
```

#### 29. 15. 3Sum

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

Example 1:

Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
Example 2:

Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
Example 3:

Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        multiset<int> st(nums.begin(), nums.end());
        set<vector<int>> res;
        sort(nums.begin(), nums.end());
        st.erase(st.find(nums.front()));
        for (int j = 1; j < nums.size(); j++) {
            st.erase(st.find(nums[j]));
            for (int i = 0; i < j; i++) {
                if (st.find(-1 * (nums[i] + nums[j])) != st.end()) {
                    res.insert({nums[i], nums[j], -1 * (nums[i] + nums[j])});
                }
            }
        }
        return vector<vector<int>>(res.begin(), res.end());
    }
};
```

#### 30. 209. Minimum Size Subarray Sum

Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray
 whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

Example 1:

Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
Example 2:

Input: target = 4, nums = [1,4,4]
Output: 1
Example 3:

Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int res = INT_MAX;
        for (int i = 0, j = 0, curr = 0; j < nums.size(); j++) {
            curr += nums[j];
            while (curr >= target) {
                curr -= nums[i];
                res = min(res, j - i + 1);
                i++;
            }
        }
        return res == INT_MAX ? 0 : res;
    }
};
```

#### 31. 3. Longest Substring Without Repeating Characters

Given a string s, find the length of the longest 
substring
 without repeating characters. 

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int res = 0;
        set<char> st;
        for (int i = 0, j = 0; j < s.size(); j++) {
            if (st.find(s[j]) != st.end()) {
                while (s[i] != s[j]) {
                    st.erase(st.find(s[i++]));
                }
                i++;
            }
            st.insert(s[j]);
            res = max(res, j - i + 1);
        }
        return res;
    }
};
```

#### 32. 30. Substring with Concatenation of All Words

You are given a string s and an array of strings words. All the strings of words are of the same length.

A concatenated string is a string that exactly contains all the strings of any permutation of words concatenated.

For example, if words = ["ab","cd","ef"], then "abcdef", "abefcd", "cdabef", "cdefab", "efabcd", and "efcdab" are all concatenated strings. "acdbef" is not a concatenated string because it is not the concatenation of any permutation of words.
Return an array of the starting indices of all the concatenated substrings in s. You can return the answer in any order.


Example 1:

Input: s = "barfoothefoobarman", words = ["foo","bar"]

Output: [0,9]

Explanation:

The substring starting at 0 is "barfoo". It is the concatenation of ["bar","foo"] which is a permutation of words.
The substring starting at 9 is "foobar". It is the concatenation of ["foo","bar"] which is a permutation of words.

Example 2:

Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

Output: []

Explanation:

There is no concatenated substring.

Example 3:

Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

Output: [6,9,12]

Explanation:

The substring starting at 6 is "foobarthe". It is the concatenation of ["foo","bar","the"].
The substring starting at 9 is "barthefoo". It is the concatenation of ["bar","the","foo"].
The substring starting at 12 is "thefoobar". It is the concatenation of ["the","foo","bar"].

```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> v;
        unordered_map<string, int> m;
        for (auto x : words) m[x]++;

        int l = words[0].length();
        int len = words.size() * l;
        len = s.length() - len;
        if (len < 0) return v;
        for (int i = 0; i <= len; i++) {
            unordered_map<string, int> mp = m;
            int c = 0;
            for (int j = i; j < s.size(); j += l) {
                string x = s.substr(j, l);
                if (mp[x] > 0) {
                    c++;
                    mp[x]--;
                } 
                else break;
            }
            if (c == words.size()) v.push_back(i);
        }
        return v;
    }
};
```

#### 33. 76. Minimum Window Substring

Given two strings s and t of lengths m and n respectively, return the minimum window 
substring
 of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.


Example 1:

Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
Example 2:

Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
Example 3:

Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

```cpp
class Solution {
public:
    bool valid(const vector<int>& at, const vector<int>& At, 
               const vector<int>& as,const vector<int>& As) {
        for (int i = 0; i < 26; i++) {
            if (at[i] > as[i]) return false;
            if (At[i] > As[i]) return false;
        }
        return true;
    }

    string minWindow(string s, string t) {
        set<char> st(t.begin(), t.end());
        vector<int> at(26), At(26), as(26), As(26);
        for (const auto& c: t) {
            if (c >= 'a' && c <= 'z') at[c - 'a']++;
            else At[c - 'A']++;
        }
        int minSize = INT_MAX, l = -1, r = -1;
        for (int i = 0, j = 0; j < s.size(); j++) {
            if (st.find(s[j]) != st.end()) {
                if (s[j] >= 'a' && s[j] <= 'z') as[s[j] - 'a']++;
                else As[s[j] - 'A']++;
            }
            while (valid(at, At, as, As)) {
                if (j - i + 1 < minSize) {
                    l = i, r = j;
                    minSize = j - i + 1;
                }
                if (st.find(s[i]) != st.end()) {
                    if (s[i] >= 'a' && s[i] <= 'z') as[s[i] - 'a']--;
                    else As[s[i] - 'A']--;
                }
                i++;
            }
        }
        return minSize == INT_MAX ? "" : s.substr(l, r - l + 1);
    }
};
```

#### 34. 36. Valid Sudoku

Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
Note:

A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.
 

Example 1:


Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
Example 2:

Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        // check rows
        for (int i = 0; i < board.size(); i++) {
            vector<int> curr(10);
            for (int j = 0; j < board[0].size(); j++) {
                if (board[i][j] == '.') continue;
                if (++curr[board[i][j] - '0'] > 1) return false; 
            }
        }
        // check columns
        for (int j = 0; j < board[0].size(); j++) {
            vector<int> curr(10);
            for (int i = 0; i < board.size(); i++) {
                if (board[i][j] == '.') continue;
                if (++curr[board[i][j] - '0'] > 1) return false;
            }
        }
        // check 3X3 grid
        for (int i = 0; i < board.size(); i += 3) {
            for (int j = 0; j < board[0].size(); j += 3) {
                vector<int> curr(10);
                for (int a = i; a < i + 3; a++) {
                    for (int b = j; b < j + 3; b++) {
                        if (board[a][b] == '.') continue;
                        if (++curr[board[a][b] - '0'] > 1) return false;
                    }
                }
            }
        }
        return true;
    }
};
```

#### 35. 54. Spiral Matrix

Given an m x n matrix, return all elements of the matrix in spiral order. 

Example 1:


Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
Example 2:


Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int n = matrix[0].size() - 1;
        int m = matrix.size() - 1;
        int top = 0;
        int right = n;
        int left = 0;
        int bottom = m;

        vector<int> res;

        while(top <= bottom && left <= right){
            // Top row
            for(int i = left; i <= right; i++){
                res.push_back(matrix[top][i]);
            }
            top++;

            // Right column
            for(int i = top; i <= bottom; i++){
                res.push_back(matrix[i][right]);
            }
            right--;

            if(top <= bottom){
                // Bottom row
                for(int i = right; i >= left; i--){
                    res.push_back(matrix[bottom][i]);
                }
                bottom--;
            }

            if(left <= right){
                // Left column
                for(int i = bottom; i >= top; i--){
                    res.push_back(matrix[i][left]);
                }
                left++;
            }
        }
        return res;
    }
};
```

#### 36. 48. Rotate Image

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.


Example 1:


Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
Example 2:


Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        // Transpose of matrix
        for (int i = 0; i < matrix.size(); i++) {
            for (int j = i; j < matrix.size(); j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
        // Reverse each row
        for (int i = 0; i < matrix.size(); i++) {
            for (int j = 0; j < matrix.size() / 2; j++) {
                swap(matrix[i][j], matrix[i][matrix.size() - j - 1]);
            }
        }
    }
};
```

#### 37. 73. Set Matrix Zeroes

Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's.

You must do it in place.

Example 1:


Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
Example 2:


Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        if (matrix.empty())
            return;

        int m = matrix.size();
        int n = matrix[0].size();

        bool firstRowHasZero = false;
        bool firstColHasZero = false;

        // Check if the first row has a zero
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                firstRowHasZero = true;
                break;
            }
        }

        // Check if the first column has a zero
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstColHasZero = true;
                break;
            }
        }

        // Use the first row and first column to mark zeros
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0; // Mark the first column
                    matrix[0][j] = 0; // Mark the first row
                }
            }
        }

        // Zero out rows based on marks in the first column
        for (int i = 1; i < m; i++) {
            if (matrix[i][0] == 0) {
                for (int j = 1; j < n; j++) {
                    matrix[i][j] = 0;
                }
            }
        }

        // Zero out columns based on marks in the first row
        for (int j = 1; j < n; j++) {
            if (matrix[0][j] == 0) {
                for (int i = 1; i < m; i++) {
                    matrix[i][j] = 0;
                }
            }
        }

        // Zero out the first row if needed
        if (firstRowHasZero) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }

        // Zero out the first column if needed
        if (firstColHasZero) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
    }
};
```

#### 38. 289. Game of Life

According to Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an m x n grid of cells, where each cell has an initial state: live (represented by a 1) or dead (represented by a 0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

Any live cell with fewer than two live neighbors dies as if caused by under-population.
Any live cell with two or three live neighbors lives on to the next generation.
Any live cell with more than three live neighbors dies, as if by over-population.
Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
The next state of the board is determined by applying the above rules simultaneously to every cell in the current state of the m x n grid board. In this process, births and deaths occur simultaneously.

Given the current state of the board, update the board to reflect its next state.

Note that you do not need to return anything.


Example 1:


Input: board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
Output: [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
Example 2:


Input: board = [[1,1],[1,0]]
Output: [[1,1],[1,1]]

```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        vector<vector<int>>ans(board);
        vector<vector<int>>dir={{-1,-1},{-1,0},{-1,1},
                                    {0,-1},{0,1},
                                {1,1},{1,0},{1,-1}};
        int n=board.size();
        int m=board[0].size();
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                int ableToLive=0;
                for(int k=0;k<8;k++){
                    if(((i+dir[k][0]>=0) && (i+dir[k][0]<n)) && 
                        (j+dir[k][1]>=0) && (j+dir[k][1]<m)){
                        
                        int row=i+dir[k][0], col=j+dir[k][1];
                        if(ans[row][col]==1) ableToLive++;
                    }
                }
                if(ans[i][j]==1 && (ableToLive<2 || ableToLive>3)) board[i][j]=0;

                else if(ans[i][j]==0 && ableToLive==3) board[i][j]=1;
                else if(ans[i][j]==1 && (ableToLive==2 || ableToLive==3 )) board[i][j]=1;
            }
        }
    }
};
```

#### 39. 383. Ransom Note

Given two strings ransomNote and magazine, return true if ransomNote can be constructed by using the letters from magazine and false otherwise.

Each letter in magazine can only be used once in ransomNote.
 

Example 1:

Input: ransomNote = "a", magazine = "b"
Output: false
Example 2:

Input: ransomNote = "aa", magazine = "ab"
Output: false
Example 3:

Input: ransomNote = "aa", magazine = "aab"
Output: true

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        vector<int> fr(26), fm(26);
        for (const auto& ch: ransomNote) fr[ch - 'a']++;
        for (const auto& ch: magazine) fm[ch - 'a']++;
        for (int i = 0; i < 26; i++) if(fr[i] > fm[i]) return false;
        return true;
    }
};
```

#### 40. 205. Isomorphic Strings

Given two strings s and t, determine if they are isomorphic.

Two strings s and t are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

Example 1:

Input: s = "egg", t = "add"

Output: true

Explanation:

The strings s and t can be made identical by:

Mapping 'e' to 'a'.
Mapping 'g' to 'd'.
Example 2:

Input: s = "foo", t = "bar"

Output: false

Explanation:

The strings s and t can not be made identical as 'o' needs to be mapped to both 'a' and 'r'.

Example 3:

Input: s = "paper", t = "title"

Output: true

```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char, char> st, ts;
        for (int i = 0; i < s.size(); i++) {
            if (st.find(s[i]) != st.end() && st[s[i]] != t[i]) return false;
            if (ts.find(t[i]) != ts.end() && ts[t[i]] != s[i]) return false;
            st[s[i]] = t[i];
            ts[t[i]] = s[i];
        }
        return true;
    }
};
```

#### 41. 290. Word Pattern

Given a pattern and a string s, find if s follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in s. Specifically:

Each letter in pattern maps to exactly one unique word in s.
Each unique word in s maps to exactly one letter in pattern.
No two letters map to the same word, and no two words map to the same letter.
 

Example 1:

Input: pattern = "abba", s = "dog cat cat dog"

Output: true

Explanation:

The bijection can be established as:

'a' maps to "dog".
'b' maps to "cat".
Example 2:

Input: pattern = "abba", s = "dog cat cat fish"

Output: false

Example 3:

Input: pattern = "aaaa", s = "dog cat cat dog"

Output: false

```cpp
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        unordered_map<char, string> ps;
        unordered_map<string, char> sp;
        istringstream is(s);
        string word;
        int i = 0;
        while (is >> word) {
            if (ps.find(pattern[i]) != ps.end() && ps[pattern[i]] != word) return false;
            if (sp.find(word) != sp.end() && sp[word] != pattern[i]) return false;
            ps[pattern[i]] = word;
            sp[word] = pattern[i];
            i++;
        }
        return i == pattern.size();
    }
};
```

#### 42. 242. Valid Anagram

Given two strings s and t, return true if t is an 
anagram
 of s, and false otherwise.


Example 1:

Input: s = "anagram", t = "nagaram"

Output: true

Example 2:

Input: s = "rat", t = "car"

Output: false

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
       vector<int> fs(26), ft(26);
       for (const auto& ch: s) fs[ch - 'a']++;
       for (const auto& ch: t) ft[ch - 'a']++;
       for (int i = 0; i < 26; i++) if (fs[i] != ft[i]) return false;
       return true; 
    }
};
```

#### 43. 1. Two Sum

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.
 

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]
Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> mp;
        for (int i = 0; i < nums.size(); i++) {
            if (mp.find(target - nums[i]) != mp.end()) return {mp[target - nums[i]], i};
            mp[nums[i]] = i;
        }
        return {-1, -1};
    }
};
```

#### 44. 49. Group Anagrams

Given an array of strings strs, group the 
anagrams
 together. You can return the answer in any order.
 

Example 1:

Input: strs = ["eat","tea","tan","ate","nat","bat"]

Output: [["bat"],["nat","tan"],["ate","eat","tea"]]

Explanation:

There is no string in strs that can be rearranged to form "bat".
The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.
Example 2:

Input: strs = [""]

Output: [[""]]

Example 3:

Input: strs = ["a"]

Output: [["a"]]

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> groups;
        for (const auto& str: strs) {
            string s = str;
            sort(s.begin(), s.end());
            groups[s].push_back(str);
        }
        vector<vector<string>> res;
        for (const auto& [_, group]: groups) res.push_back(group);
        return res;
    }
};
```

#### 45. 202. Happy Number

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.


Example 1:

Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
Example 2:

Input: n = 2
Output: false

```cpp
class Solution {
public:
    unordered_set <int> st;
    bool isHappy(int n) {
        if(n == 1) return true;
        
        if (st.find(n) != st.end()) return false;
        st.insert(n);

        int sum = 0;
        while(n!=0){
            int digit = n % 10;
            sum += digit * digit;
            n = n/10;
        }
        return isHappy(sum);
    }
};
```

#### 46. 219. Contains Duplicate II

Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k. 

Example 1:

Input: nums = [1,2,3,1], k = 3
Output: true
Example 2:

Input: nums = [1,0,1,1], k = 1
Output: true
Example 3:

Input: nums = [1,2,3,1,2,3], k = 2
Output: false

```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        for (int i = 0; i < nums.size(); i++) {
            if (mp.find(nums[i]) != mp.end() && i - mp[nums[i]] <= k) return true;
            mp[nums[i]] = i;
        }
        return false;
    }
};
```

#### 47. 128. Longest Consecutive Sequence

Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.
 

Example 1:

Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
Example 2:

Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9


```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> st(nums.begin(), nums.end());
        int res = 0;
        for(auto num: nums){
            if(st.find(num - 1) == st.end()) {
                int curr = 1;
                while(st.find(num + 1) != st.end()){
                    num++;
                    curr++;
                }
                res = max(res, curr);
            }
        }
        return res;
    }
};
```

#### 48. 228. Summary Ranges

You are given a sorted unique integer array nums.

A range [a,b] is the set of all integers from a to b (inclusive).

Return the smallest sorted list of ranges that cover all the numbers in the array exactly. That is, each element of nums is covered by exactly one of the ranges, and there is no integer x such that x is in one of the ranges but not in nums.

Each range [a,b] in the list should be output as:

"a->b" if a != b
"a" if a == b
 

Example 1:

Input: nums = [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: The ranges are:
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
Example 2:

Input: nums = [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: The ranges are:
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"

```cpp
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        if (nums.empty()) return {};
        vector<string> res;
        int start = nums.front(), prev = nums.front();
        string str = "";
        for (const auto& num: nums) {
            if (prev + 1 != num) {
                if (str != "") {
                    if (start == prev) res.push_back(str);
                    else res.push_back(str + "->" + to_string(prev));
                }
                str = to_string(num);
                start = num;
            }
            prev = num;
        }
        if (str != "") {
            if (start == prev) res.push_back(str);
            else res.push_back(str + "->" + to_string(prev));
        }
        return res;
    }
};
```

#### 49. 56. Merge Intervals

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.


Example 1:

Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
Example 2:

Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
            return a[0] == b[0] ? a[1] < b[1] : a[0] < b[0];
        });
        vector<vector<int>> res;
        int s = -1, e = -1;
        for (const auto& interval: intervals) {
            if (interval[0] > e) {
                if (s != -1) res.push_back({s, e});
                s = interval[0];
            }
            e = max(e, interval[1]);
        }
        res.push_back({s, e});
        return res;
    }
};
```

#### 50. 57. Insert Interval

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

Note that you don't need to modify intervals in-place. You can make a new array and return it.
 

Example 1:

Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
Example 2:

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
            return a[0] == b[0] ? a[1] < b[1] : a[0] < b[0];
        });
        vector<vector<int>> res;
        int s = -1, e = -1;
        for (const auto& interval: intervals) {
            if (interval[0] >= newInterval[0]) {
                if (newInterval[0] > e) {
                    if (s != -1) res.push_back({s, e});
                    s = newInterval[0];
                }
                e = max(e, newInterval[1]);
                // Avoid new interval being inserted twice
                newInterval[0] = INT_MAX;
            }
            if (interval[0] > e) {
                if (s != -1) res.push_back({s, e});
                s = interval[0];
            }
            e = max(e, interval[1]);
        }
        if (newInterval[0] > e) {
            if (s != -1) res.push_back({s, e});
            if (newInterval[0] != INT_MAX) res.push_back(newInterval);
        }
        else res.push_back({s, max(e, newInterval[1])});
        return res;
    }
};
```

#### 51. 452. Minimum Number of Arrows to Burst Balloons

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array points where points[i] = [xstart, xend] denotes a balloon whose horizontal diameter stretches between xstart and xend. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis. A balloon with xstart and xend is burst by an arrow shot at x if xstart <= x <= xend. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array points, return the minimum number of arrows that must be shot to burst all balloons.
 

Example 1:

Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
Example 2:

Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
Explanation: One arrow needs to be shot for each balloon for a total of 4 arrows.
Example 3:

Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 2, bursting the balloons [1,2] and [2,3].
- Shoot an arrow at x = 4, bursting the balloons [3,4] and [4,5].

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        // sort points with x_end as we have to use an arrow at the end of every 
        // baloon row
        sort(points.begin(), points.end(), [](const auto& a, const auto& b) {
            return a[1] == b[1] ? a[0] < b[0] : a[1] < b[1];
        });
        int res = 0;
        long long prev = LONG_LONG_MIN;
        for (const auto& point: points) {
            if (point[0] > prev) {
                res++;
                prev = point[1];
            }
        }
        return res;
    }
};
```

#### 52. 20. Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:

Input: s = "()"

Output: true

Example 2:

Input: s = "()[]{}"

Output: true

Example 3:

Input: s = "(]"

Output: false

Example 4:

Input: s = "([])"

Output: true

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (char cur : s) {
            if (!st.empty()) {
                char last = st.top();
                if (isPair(last, cur)) {
                    st.pop();
                    continue;
                }
            }
            st.push(cur);
        }
        return st.empty();
    }

private:
    bool isPair(char last, char cur) {
        return (last == '(' && cur == ')') || (last == '{' && cur == '}') ||
               (last == '[' && cur == ']');
    }
};
```

#### 53. 71. Simplify Path

You are given an absolute path for a Unix-style file system, which always begins with a slash '/'. Your task is to transform this absolute path into its simplified canonical path.

The rules of a Unix-style file system are as follows:

A single period '.' represents the current directory.
A double period '..' represents the previous/parent directory.
Multiple consecutive slashes such as '//' and '///' are treated as a single slash '/'.
Any sequence of periods that does not match the rules above should be treated as a valid directory or file name. For example, '...' and '....' are valid directory or file names.
The simplified canonical path should follow these rules:

The path must start with a single slash '/'.
Directories within the path must be separated by exactly one slash '/'.
The path must not end with a slash '/', unless it is the root directory.
The path must not have any single or double periods ('.' and '..') used to denote current or parent directories.
Return the simplified canonical path.

 

Example 1:

Input: path = "/home/"

Output: "/home"

Explanation:

The trailing slash should be removed.

Example 2:

Input: path = "/home//foo/"

Output: "/home/foo"

Explanation:

Multiple consecutive slashes are replaced by a single one.

Example 3:

Input: path = "/home/user/Documents/../Pictures"

Output: "/home/user/Pictures"

Explanation:

A double period ".." refers to the directory up a level (the parent directory).

Example 4:

Input: path = "/../"

Output: "/"

Explanation:

Going one level up from the root directory is not possible.

Example 5:

Input: path = "/.../a/../b/c/../d/./"

Output: "/.../b/d"

Explanation:

"..." is a valid name for a directory in this problem.

```cpp
class Solution {
public:
    string simplifyPath(string path) {
        stack<string> st;
        istringstream iss(path);
        string word;
        while (getline(iss, word, '/')) {
            if (word == ".") continue;
            if (word == "..") if (!st.empty()) st.pop();
            if (!word.empty() && word != "..") st.push(word);
        }
        string res = "";
        while (!st.empty()) {
            string str = st.top();
            // reverse every word so that at the end we can reverse the whole res
            reverse(str.begin(), str.end());
            res += str + "/";
            st.pop();
        }
        reverse(res.begin(), res.end());
        return res == "" ? "/" : res;
    }
};
```

#### 54. 155. Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

MinStack() initializes the stack object.
void push(int val) pushes the element val onto the stack.
void pop() removes the element on the top of the stack.
int top() gets the top element of the stack.
int getMin() retrieves the minimum element in the stack.
You must implement a solution with O(1) time complexity for each function.
 

Example 1:

Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2

```cpp
class MinStack {
private:
    stack<pair<int, int>> st;
public:
    MinStack() {
        
    }
    
    void push(int val) {
        if (st.empty()) st.push({val, val});
        else st.push({val, min(val, st.top().second)});
    }
    
    void pop() {
        st.pop();
    }
    
    int top() {
        return st.top().first;
    }
    
    int getMin() {
        return st.top().second;
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

#### 55. 150. Evaluate Reverse Polish Notation

You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

The valid operators are '+', '-', '*', and '/'.
Each operand may be an integer or another expression.
The division between two integers always truncates toward zero.
There will not be any division by zero.
The input represents a valid arithmetic expression in a reverse polish notation.
The answer and all the intermediate calculations can be represented in a 32-bit integer.
 

Example 1:

Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
Example 2:

Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
Example 3:

Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        for (const auto& token: tokens) {
            if (token == "+") {
                int y = st.top();
                st.pop();
                int x = st.top();
                st.pop();
                st.push(x + y);
            }
            else if (token == "-") {
                int y = st.top();
                st.pop();
                int x = st.top();
                st.pop();
                st.push(x - y);
            }
            else if (token == "*") {
                int y = st.top();
                st.pop();
                int x = st.top();
                st.pop();
                st.push(x * y);
            }
            else if (token == "/") {
                int y = st.top();
                st.pop();
                int x = st.top();
                st.pop();
                st.push(x / y);
            }
            else st.push(stoi(token));
        }
        return st.top();
    }
};
```

#### 56. 108. Convert Sorted Array to Binary Search Tree

Given an integer array nums where the elements are sorted in ascending order, convert it to a 
height-balanced
 binary search tree.


Example 1:


Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:

Example 2:


Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* buildBST(int l, int r, const vector<int>& nums) {
        if (l > r) return nullptr;
        int m = (l + r) / 2;
        TreeNode* root = new TreeNode(nums[m]);
        root->left = buildBST(l, m - 1, nums);
        root->right = buildBST(m + 1, r, nums);
        return root;
    }

    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return buildBST(0, nums.size() - 1, nums);
    }
};
```

#### 57. 148. Sort List

Given the head of a linked list, return the list after sorting it in ascending order.
 

Example 1:


Input: head = [4,2,1,3]
Output: [1,2,3,4]
Example 2:


Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
Example 3:

Input: head = []
Output: []

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
private:
    ListNode* findMid(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head->next;

        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode* merge(ListNode* left, ListNode* right) {

        if (left == NULL) {
            return right;
        }

        if (right == NULL) {

            return right;
        }

        ListNode* ans = new ListNode(-1);
        ListNode* temp = ans;

        while (left != NULL && right != NULL) {

            if (left->val < right->val) {
                temp->next = left;
                temp = left;
                left = left->next;
            } else {

                temp->next = right;
                temp = right;
                right = right->next;
            }
        }

        while (left != NULL) {
            temp->next = left;
            temp = left;
            left = left->next;
        }

        while (right != NULL) {
            temp->next = right;
            temp = right;
            right = right->next;
        }

        ans = ans->next;
        return ans;
    }

public:
    ListNode* sortList(ListNode* head) {

        if (head == NULL || head->next == NULL) {

            return head;
        }

        ListNode* mid = findMid(head);

        ListNode* left = head;
        ListNode* right = mid->next;
        mid->next = NULL;

        // recursive calls

        left = sortList(left);
        right = sortList(right);

        ListNode* result = merge(left, right);

        return result;
    }
};
```

#### 58. 427. Construct Quad Tree

Given a n * n matrix grid of 0's and 1's only. We want to represent grid with a Quad-Tree.

Return the root of the Quad-Tree representing grid.

A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

val: True if the node represents a grid of 1's or False if the node represents a grid of 0's. Notice that you can assign the val to True or False when isLeaf is False, and both are accepted in the answer.
isLeaf: True if the node is a leaf node on the tree or False if the node has four children.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
We can construct a Quad-Tree from a two-dimensional area using the following steps:

If the current grid has the same value (i.e all 1's or all 0's) set isLeaf True and set val to the value of the grid and set the four children to Null and stop.
If the current grid has different values, set isLeaf to False and set val to any value and divide the current grid into four sub-grids as shown in the photo.
Recurse for each of the children with the proper sub-grid.

If you want to know more about the Quad-Tree, you can refer to the wiki.

Quad-Tree format:

You don't need to read this section for solving the problem. This is only if you want to understand the output format here. The output represents the serialized format of a Quad-Tree using level order traversal, where null signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list [isLeaf, val].

If the value of isLeaf or val is True we represent it as 1 in the list [isLeaf, val] and if the value of isLeaf or val is False we represent it as 0.

 

Example 1:


Input: grid = [[0,1],[1,0]]
Output: [[0,1],[1,0],[1,1],[1,1],[1,0]]
Explanation: The explanation of this example is shown below:
Notice that 0 represents False and 1 represents True in the photo representing the Quad-Tree.

Example 2:



Input: grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
Output: [[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
Explanation: All values in the grid are not the same. We divide the grid into four sub-grids.
The topLeft, bottomLeft and bottomRight each has the same value.
The topRight have different values so we divide it into 4 sub-grids where each has the same value.
Explanation is shown in the photo below:


```cpp
/*
// Definition for a QuadTree node.
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;
    
    Node() {
        val = false;
        isLeaf = false;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }
    
    Node(bool _val, bool _isLeaf) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }
    
    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};
*/

class Solution {
public:
    Node* buildQT(int r1, int c1, int r2, int c2, const vector<vector<int>>& grid) {
        int tot = 0;
        for (int i = r1; i <= r2; i++) {
            for (int j = c1; j <= c2; j++) {
                tot += grid[i][j];
            }
        }

        if ((r2 - r1 + 1) * (c2 - c1 + 1) == tot || tot == 0) {
            return new Node(tot == 0 ? 0 : 1, true);
        }
        
        Node* root = new Node(grid[r1][c1], false);
        int mr = (r1 + r2) / 2;
        int mc = (c1 + c2) / 2;
        root->topLeft = buildQT(r1, c1, mr, mc, grid);
        root->topRight = buildQT(r1, mc + 1, mr, c2, grid);
        root->bottomLeft = buildQT(mr + 1, c1, r2, mc, grid);
        root->bottomRight = buildQT(mr + 1, mc + 1, r2, c2, grid);
        return root;
    }

    Node* construct(vector<vector<int>>& grid) {
        return buildQT(0, 0, grid.size() - 1, grid.size() - 1, grid);        
    }
};
```

#### 59. 23. Merge k Sorted Lists

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.
 

Example 1:

Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
Example 2:

Input: lists = []
Output: []
Example 3:

Input: lists = [[]]
Output: []

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoList(ListNode* head1, ListNode* head2)  {
        if (!head1) return head2;
        if (!head2) return head1;
        ListNode* ans = new ListNode(-1);
        ListNode* ptr = ans;
        while (head1 && head2) {
            if (head1->val < head2->val) {
                ptr->next = head1;
                ptr = ptr->next;
                head1 = head1->next;
            } 
            else {
                ptr->next = head2;
                ptr = ptr->next;
                head2 = head2->next;
            }
        }
        while (head1) {
            ptr->next = head1;
            ptr = ptr->next;
            head1 = head1->next;
        }
        while (head2) {
            ptr->next = head2;
            ptr = ptr->next;
            head2 = head2->next;
        }
        return ans->next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.size() == 0) return NULL;
        ListNode* head1 = lists[0];
        for (int i = 1; i < lists.size(); i++) head1 = mergeTwoList(head1, lists[i]);
        return head1;
    }
};
```

#### 60. 53. Maximum Subarray
Solved
Medium
Topics
Companies
Given an integer array nums, find the 
subarray
 with the largest sum, and return its sum.
 

Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
Example 2:

Input: nums = [1]
Output: 1
Explanation: The subarray [1] has the largest sum 1.
Example 3:

Input: nums = [5,4,-1,7,8]
Output: 23
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = -1e9, curr = -1e9;
        for (const auto& num: nums) {
            if (curr + num < num) curr = num;
            else curr += num;
            res = max(res, curr);
        }
        return res;
    }
};
```

#### 61. 918. Maximum Sum Circular Subarray

Given a circular integer array nums of length n, return the maximum possible sum of a non-empty subarray of nums.

A circular array means the end of the array connects to the beginning of the array. Formally, the next element of nums[i] is nums[(i + 1) % n] and the previous element of nums[i] is nums[(i - 1 + n) % n].

A subarray may only include each element of the fixed buffer nums at most once. Formally, for a subarray nums[i], nums[i + 1], ..., nums[j], there does not exist i <= k1, k2 <= j with k1 % n == k2 % n.
 

Example 1:

Input: nums = [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3.
Example 2:

Input: nums = [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10.
Example 3:

Input: nums = [-3,-2,-3]
Output: -2
Explanation: Subarray [-2] has maximum sum -2.

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        // So there are two case.
        // Case 1. The first is that the subarray take only a middle part, and
        // we know how to find the max subarray sum. 
        // Case2. The second is that
        // the subarray take a part of head array and a part of tail array. We
        // can transfer this case to the first one. The maximum result equals to
        // the total sum minus the minimum subarray sum.
        int total = 0, maxSum = nums.front(), curMax = 0, minSum = nums.front(), curMin = 0;
        for (const auto& num: nums) {
            curMax = max(curMax + num, num);
            maxSum = max(maxSum, curMax);
            curMin = min(curMin + num, num);
            minSum = min(minSum, curMin);
            total += num;
        }
        return maxSum > 0 ? max(maxSum, total - minSum) : maxSum;
    }
};
```

#### 62. 35. Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.
 

Example 1:

Input: nums = [1,3,5,6], target = 5
Output: 2
Example 2:

Input: nums = [1,3,5,6], target = 2
Output: 1
Example 3:

Input: nums = [1,3,5,6], target = 7
Output: 4

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        // return lower_bound(nums.begin(), nums.end(), target) - nums.begin();
        // standard binary search implementation
        int l = -1, r = nums.size();
        while (r - l > 1) {
            int m = l + (r - l) / 2;
            if (nums[m] > target) r = m;
            else l = m;
        }
        return l != -1 && nums[l] == target ? l : r;
    }
};
```

#### 63. 74. Search a 2D Matrix

You are given an m x n integer matrix matrix with the following two properties:

Each row is sorted in non-decreasing order.
The first integer of each row is greater than the last integer of the previous row.
Given an integer target, return true if target is in matrix or false otherwise.

You must write a solution in O(log(m * n)) time complexity.
 

Example 1:

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
Example 2:


Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size(), m = matrix[0].size();
        int l = -1, r = n * m;
        while (r - l > 1) {
            int mm = l + (r - l) / 2;
            int row = mm / m;
            int col = mm % m;
            if (matrix[row][col] == target) return true;
            else if (matrix[row][col] < target) l = mm;
            else r = mm;
        }
        return false;
    }
};
```

#### 64. 162. Find Peak Element

A peak element is an element that is strictly greater than its neighbors.

Given a 0-indexed integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that nums[-1] = nums[n] = -∞. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in O(log n) time.
 

Example 1:

Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
Example 2:

Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        // Given that nums[i] != nums[i + 1] for all valid i then
        // there must be atleast one peak
        int l = 0, r = nums.size();
        while (l <= r) {
            int m = l + (r - l) / 2;
            if ((m - 1 < 0 || nums[m - 1] < nums[m]) && (m + 1 >= nums.size() || nums[m] > nums[m + 1])) {
                return m;
            }
            if (m - 1 >= 0 && nums[m - 1] > nums[m]) r = m - 1;
            else l = m + 1;
        }
        return -1;
    }
};
```

#### 65. 33. Search in Rotated Sorted Array

There is an integer array nums sorted in ascending order (with distinct values).

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

```cpp
class Solution {
public:
    int pivot(const vector<int>& nums) {
        if (nums.front() < nums.back()) return 0;
        int l = 0, r = nums.size() - 1;
        while (r - l > 1) {
            int m = l + (r - l) / 2;
            if (nums[m] > nums[r]) l = m;
            else r = m;
        }
        return r;
    }

    int search(const vector<int>& nums, int l, int r, int target) {
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (nums[m] == target) return m;
            else if (nums[m] < target) l = m + 1;
            else r = m - 1;
        }
        return -1;
    }

    int search(vector<int>& nums, int target) {
        // find the pivot where the rotation has made
        int p = pivot(nums);
        if (p == 0) return search(nums, 0, nums.size() - 1, target);
        // search in both halves
        int ll = search(nums, 0, p - 1, target), rr = search(nums, p, nums.size() - 1, target);
        return max(ll, rr);
    }
};
```

#### 66. 34. Find First and Last Position of Element in Sorted Array

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

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l = -1, r = nums.size();
        while (r - l > 1) {
            int m = l + (r - l) / 2;
            if (nums[m] >= target) r = m;
            else l = m;
        }
        if (r == nums.size() || nums[r] != target) return {-1, -1};
        vector<int> res = {r, -1};
        l = -1, r = nums.size();
        while (r - l > 1) {
            int m = l + (r - l) / 2;
            if (nums[m] <= target) l = m;
            else r = m;
        }
        res[1] = l;
        return res;
    }
};
```

#### 67. 153. Find Minimum in Rotated Sorted Array

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.
 

Example 1:

Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
Example 2:

Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
Example 3:

Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times.

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        if (nums.front() < nums.back()) return nums.front();
        int l = 0, r = nums.size() - 1;
        while (r - l > 1) {
            int m = l + (r - l) / 2;
            if (nums[m] > nums[r]) l = m;
            else r = m;
        }
        return nums[r];
    }
};
```



#### 68. 4. Median of Two Sorted Arrays

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).
 

Example 1:

Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
Example 2:

Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int> &nums1, vector<int> &nums2) {
        int n1 = nums1.size(), n2 = nums2.size();
        
        if (n1 > n2) return findMedianSortedArrays(nums2, nums1);
        
        int n = n1 + n2;
        int left = (n1 + n2 + 1) / 2; 
        int low = 0, high = n1;
        
        while (low <= high) {
            int mid1 = (low + high) >> 1; 
            int mid2 = left - mid1;
            
            int l1 = INT_MIN, l2 = INT_MIN, r1 = INT_MAX, r2 = INT_MAX;
            
            if (mid1 < n1)
                r1 = nums1[mid1];
            if (mid2 < n2)
                r2 = nums2[mid2];
            if (mid1 - 1 >= 0)
                l1 = nums1[mid1 - 1];
            if (mid2 - 1 >= 0)
                l2 = nums2[mid2 - 1];
            
            if (l1 <= r2 && l2 <= r1) {
                if (n % 2 == 1) return max(l1, l2);
                else return ((double)(max(l1, l2) + min(r1, r2))) / 2.0;
            }
            else if (l1 > r2) {
                high = mid1 - 1;
            }
            else {
                low = mid1 + 1;
            }
        }
        return 0; 
    }
};

```


#### 69. 215. Kth Largest Element in an Array

Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

Can you solve it without sorting?
 

Example 1:

Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
Example 2:

Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (const auto& num: nums) {
            pq.push(num);
            while (pq.size() > k) pq.pop();
        }
        return pq.top();
    }
};
```


#### 70. 502. IPO

Suppose LeetCode will start its IPO soon. In order to sell a good price of its shares to Venture Capital, LeetCode would like to work on some projects to increase its capital before the IPO. Since it has limited resources, it can only finish at most k distinct projects before the IPO. Help LeetCode design the best way to maximize its total capital after finishing at most k distinct projects.

You are given n projects where the ith project has a pure profit profits[i] and a minimum capital of capital[i] is needed to start it.

Initially, you have w capital. When you finish a project, you will obtain its pure profit and the profit will be added to your total capital.

Pick a list of at most k distinct projects from given projects to maximize your final capital, and return the final maximized capital.

The answer is guaranteed to fit in a 32-bit signed integer.


Example 1:

Input: k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]
Output: 4
Explanation: Since your initial capital is 0, you can only start the project indexed 0.
After finishing it you will obtain profit 1 and your capital becomes 1.
With capital 1, you can either start the project indexed 1 or the project indexed 2.
Since you can choose at most 2 projects, you need to finish the project indexed 2 to get the maximum capital.
Therefore, output the final maximized capital, which is 0 + 1 + 3 = 4.
Example 2:

Input: k = 3, w = 0, profits = [1,2,3], capital = [0,1,2]
Output: 6

```cpp
class Solution {
public:
    int findMaximizedCapital(int k, int w, vector<int>& profits, vector<int>& capital) {
        int n = profits.size();
        vector<pair<int, int>> projects;
        for (int i = 0; i < n; ++i) {
            projects.emplace_back(capital[i], profits[i]);
        }
        sort(projects.begin(), projects.end());

        priority_queue<int> maxHeap;
        for (int i = 0, j = 0; j < k; ++j) {
            while (i < n && projects[i].first <= w) {
                maxHeap.push(projects[i].second);
                i++;
            }
            if (maxHeap.empty()) {
                break;
            }
            w += maxHeap.top();
            maxHeap.pop();
        }
        return w;
    }
};
```

#### 71. 373. Find K Pairs with Smallest Sums

You are given two integer arrays nums1 and nums2 sorted in non-decreasing order and an integer k.

Define a pair (u, v) which consists of one element from the first array and one element from the second array.

Return the k pairs (u1, v1), (u2, v2), ..., (uk, vk) with the smallest sums.
 

Example 1:

Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]]
Explanation: The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
Example 2:

Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [[1,1],[1,1]]
Explanation: The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

```cpp
class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        priority_queue<pair<int, pair<int, int>>> pq;
        for (int i = 0; i < nums1.size(); i++) {
            for (int j = 0; j < nums2.size(); j++) {
                int sum = nums1[i] + nums2[j];
                if (pq.size() < k) {
                    pq.push({sum, {nums1[i], nums2[j]}});
                } 
                else if (sum < pq.top().first) {
                    pq.pop();
                    pq.push({sum, {nums1[i], nums2[j]}});
                } 
                else break;
            }
        }

        vector<vector<int>> res;
        while (!pq.empty()) {
            res.push_back({pq.top().second.first, pq.top().second.second});
            pq.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```


#### 72. 295. Find Median from Data Stream

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

For example, for arr = [2,3,4], the median is 3.
For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.
Implement the MedianFinder class:

MedianFinder() initializes the MedianFinder object.
void addNum(int num) adds the integer num from the data stream to the data structure.
double findMedian() returns the median of all elements so far. Answers within 10-5 of the actual answer will be accepted.
 

Example 1:

Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0

```cpp
class MedianFinder {
private:
    priority_queue<int> left; 
    priority_queue<int, vector<int>, greater<int>> right;
public:
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        left.push(num);
        if (!right.empty()) {
            while (left.top() > right.top()) {
                int temp = left.top();
                left.pop();
                left.push(right.top());
                right.pop();
                right.push(temp);
            }
        }
        while (left.size() > right.size()) {
            right.push(left.top());
            left.pop();
        }
    }
    
    double findMedian() {
        if (right.size() > left.size()) return (double)right.top();
        return ((double)left.top() + (double)right.top()) / 2.0;
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

#### 73. 224. Basic Calculator

Given a string s representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().
 

Example 1:

Input: s = "1 + 1"
Output: 2
Example 2:

Input: s = " 2-1 + 2 "
Output: 3
Example 3:

Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23

```cpp
class Solution {
public:
    int calculate(string s) {
        long long int sum = 0;
        int sign = 1;
        stack<pair<int,int>> st;

        for(int i=0; i<s.size();i++){
            if(isdigit(s[i])){
                long long int num = 0;
                while(i<s.size() && isdigit(s[i])){
                    num = num * 10 + (s[i] - '0');
                    i++;
                }
                i--;
                sum += num * sign;
                sign = 1;
            }
            else if(s[i] == '('){
                st.push({sum, sign});
                sum = 0;
                sign = 1;
            }
            else if(s[i] == ')'){
                sum = st.top().first + (st.top().second * sum);
                st.pop();

            }
            else if(s[i] == '-'){
                sign = -1 * sign;
            }
        }
        return sum;
    }
};
```

#### 74. 17. Letter Combinations of a Phone Number

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
 

Example 1:

Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
Example 2:

Input: digits = ""
Output: []
Example 3:

Input: digits = "2"
Output: ["a","b","c"]

```cpp
class Solution {
public:
    void bt(int idx, string& curr, const string& digits, const vector<string>& mp, vector<string>& res) {
        if (idx == digits.size()) {
            res.push_back(curr);
            return;
        }

        for (const auto& c: mp[digits[idx] - '0']) {
            curr.push_back(c);
            bt(idx + 1, curr, digits, mp, res);
            curr.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return {};
        vector<string> mp = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> res;
        string curr;
        bt(0, curr, digits, mp, res);
        return res;
    }
};
```

#### 75. 77. Combinations

Given two integers n and k, return all possible combinations of k numbers chosen from the range [1, n].

You may return the answer in any order.
 

Example 1:

Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
Example 2:

Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination. 

```cpp
class Solution {
public:
    void bt(int idx, int n, int k, vector<int>& curr, vector<vector<int>>& res) {
        if (curr.size() == k) {
            res.push_back(curr);
            return;
        }

        for (int i = idx; i <= n; i++) {
            curr.push_back(i);
            bt(i + 1, n, k, curr, res);
            curr.pop_back();
        }
    }

    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> res;
        vector<int> curr;
        bt(1, n, k, curr, res);
        return res;
    }
};
```

#### 76. 46. Permutations

Given an array nums of distinct integers, return all the possible 
permutations
. You can return the answer in any order.
 

Example 1:

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
Example 2:

Input: nums = [0,1]
Output: [[0,1],[1,0]]
Example 3:

Input: nums = [1]
Output: [[1]]

```cpp
class Solution {
public:
    void bt(int idx, vector<int>& nums, vector<vector<int>>& res) {
        if (idx == nums.size()) {
            res.push_back(nums);
            return;
        }

        for (int i = idx; i < nums.size(); i++) {
            swap(nums[i], nums[idx]);
            bt(idx + 1, nums, res); 
            swap(nums[i], nums[idx]); 
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        bt(0, nums, res);
        return res;
    }
};

```

#### 77. 39. Combination Sum

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the 
frequency
 of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

 

Example 1:

Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
Example 2:

Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
Example 3:

Input: candidates = [2], target = 1
Output: []

```cpp
class Solution {
public:
    void bt(int idx, int sum, vector<int>& curr, int target, const vector<int>& candidates, vector<vector<int>>& res) {
        if (sum > target) return;
        if (sum == target) {
            res.push_back(curr);
            return;
        }

        for (int i = idx; i < candidates.size(); i++) { 
            curr.push_back(candidates[i]);
            bt(i, sum + candidates[i], curr, target, candidates, res); 
            curr.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> curr;
        bt(0, 0, curr, target, candidates, res);
        return res;
    }
};

```

#### 78. 22. Generate Parentheses
Solved
Medium
Topics
Companies
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

 

Example 1:

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
Example 2:

Input: n = 1
Output: ["()"]

```cpp
class Solution {
public:
    void bt(string& curr, int sum, int n, vector<string>& res) {
        if (curr.size() == n) {
            if (sum == 0) res.push_back(curr);
            return;
        }

        curr.push_back('(');
        bt(curr, sum + 1, n, res);
        curr.pop_back();
        if (sum > 0) {
            curr.push_back(')');
            bt(curr, sum - 1, n, res);
            curr.pop_back();
        }
    }

    vector<string> generateParenthesis(int n) {
        vector<string> res;
        int sum = 0;
        string curr;
        bt(curr, sum, 2 * n, res);
        return res;
    }
};
```

#### 79. 79. Word Search

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.
 

Example 1:


Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
Example 2:


Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
Example 3:


Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false

```cpp
class Solution {
public:

    vector<vector<int>> dir = {{-1, 0}, {0, -1}, {0, 1}, {1, 0}};

    bool f(int r, int c, int idx, const vector<vector<char>>& board, const string& word, vector<vector<bool>>& vis) {
        if (board[r][c] != word[idx]) return false;
        if (idx == word.size() - 1) return true;
        for (auto& d : dir) {
            if (r + d[0] >= 0 && r + d[0] < board.size() && c + d[1] >=0 && c + d[1] < board[0].size() && !vis[r + d[0]][c + d[1]]) {
                vis[r + d[0]][c + d[1]] = true;
                if (f(r + d[0], c + d[1], idx + 1, board, word, vis)) return true;
                vis[r + d[0]][c + d[1]] = false;
            }
        }
        return false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        vector<vector<bool>> vis(board.size(), vector<bool>(board[0].size(), false));
        for (int i = 0; i < board.size(); i++) for (int j = 0; j < board[0].size(); j++) {
            vis[i][j] = true;
            if (f(i, j, 0, board, word, vis)) return true;
            vis[i][j] = false;
        }
        return false;
    }
};
```

#### 80. 52. N-Queens II

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.
 

Example 1:


Input: n = 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown.
Example 2:

Input: n = 1
Output: 1

```cpp
class Solution {
public:
    int totalNQueens(int n) {
        vector<bool> col(n), diag(2 * n - 1), anti_diag(2 * n - 1);
        return solve(col, diag, anti_diag, 0);
    }

    int solve(vector<bool>& col, vector<bool>& diag, vector<bool>& anti_diag, int row) {
        int n = size(col), count = 0;
        if (row == n) return 1;
        for (int column = 0; column < n; column++)
            if (!col[column] && !diag[row + column] && !anti_diag[row - column + n - 1]) {
                col[column] = diag[row + column] = anti_diag[row - column + n - 1] = true;
                count += solve(col, diag, anti_diag, row + 1);
                col[column] = diag[row + column] = anti_diag[row - column + n - 1] = false;
            }
        return count;
    }
};
```

#### 81. 70. Climbing Stairs

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?


Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1) return 1;
        if (n == 2) return 2;
        int m = 2, prev = 1, curr = 2;
        while (m < n) {
            int temp = prev + curr;
            prev = curr;
            curr = temp;
            m++;
        }
        return curr;
    }
};
```

#### 82. 198. House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        // dp[i] = maximum money robbed upto ith house
        vector<int> dp(nums.size());
        for (int i = 0; i < nums.size(); i++) {
            if (i == 0) dp[i] = nums[i];
            else if (i == 1) dp[i] = max(nums[i], nums[i - 1]);
            else dp[i] = max(nums[i] + dp[i - 2], dp[i - 1]);
        }
        return dp.back();
    }
};
```

#### 83. 139. Word Break

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.
 

Example 1:

Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
Example 2:

Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
Example 3:

Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        set<string> st(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size());
        for (int i = 0; i < s.size(); i++) {
            for (int j = i; j >= 0; j--) {
                if (st.find(s.substr(j, i - j + 1)) != st.end()) {
                    dp[i] = dp[i] | (j == 0 ? true: dp[j - 1]);
                }
            }
        }
        return dp.back();
    }
};
```

#### 84. 322. Coin Change

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.
 

Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<long long> dp(amount + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (const auto& coin: coins) {
                if (coin <= i) dp[i] = min(dp[i], dp[i - coin] + 1);
            }
        }
        return dp.back() >= INT_MAX ? -1 : dp.back();
    }
};
```

#### 85. 300. Longest Increasing Subsequence

Given an integer array nums, return the length of the longest strictly increasing 
subsequence.
 

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1


```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        // dp[i] = subsequence of i + 1 length with dp[i] as the last element
        // in fact dp will be sorted
        vector<int> dp;
        for (const auto& num: nums) {
            auto lb = lower_bound(dp.begin(), dp.end(), num);
            if (lb == dp.end()) dp.push_back(num);
            else if (*lb == num) continue;
            else *lb = num;
        }
        return dp.size();
    }
};
```

#### 86. 120. Triangle

Given a triangle array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index i or index i + 1 on the next row.
 

Example 1:

Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
Example 2:

Input: triangle = [[-10]]
Output: -10

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        for (int i = 1; i < triangle.size(); i++) {
            for (int j = 0; j < triangle[i].size(); j++) {
                if (j == 0) triangle[i][j] += triangle[i - 1][j];
                else if (j == triangle[i].size() - 1) triangle[i][j] += triangle[i - 1][j - 1];
                else triangle[i][j] += min(triangle[i - 1][j], triangle[i - 1][j - 1]);
            }
        }
        return *min_element(triangle.back().begin(), triangle.back().end());
    }
};
```

#### 87. 64. Minimum Path Sum

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.
 

Example 1:


Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
Example 2:

Input: grid = [[1,2,3],[4,5,6]]
Output: 12 

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (i == 0 && j == 0) continue;
                else if (i == 0) grid[i][j] += grid[i][j - 1];
                else if (j == 0) grid[i][j] += grid[i - 1][j];
                else grid[i][j] += min(grid[i][j - 1], grid[i - 1][j]);
            }
        }
        return grid.back().back();
    }
};
```

#### 88. 63. Unique Paths II

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2 * 109.
 

Example 1:


Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
Example 2:


Input: obstacleGrid = [[0,1],[0,0]]
Output: 1

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& mat) {
        vector<vector<int>> dp(mat.size(), vector<int>(mat[0].size()));
        for (int i = 0; i < mat.size(); i++) {
            for (int j = 0; j < mat[0].size(); j++) {
                if (mat[i][j] == 0) {
                    if (i == 0 && j == 0) dp[i][j] = 1;
                    else if (i == 0) dp[i][j] = dp[i][j - 1];
                    else if (j == 0) dp[i][j] = dp[i - 1][j];
                    else dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        return dp.back().back();
    }
};
```

#### 89. 5. Longest Palindromic Substring

Given a string s, return the longest palindromic substring in s.


Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
Example 2:

Input: s = "cbbd"
Output: "bb"

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size()));
        for (int len = 1; len <= s.size(); len++) {
            if (len == 1) {
                for (int i = 0; i < s.size(); i++) {
                    dp[i][i] = true;
                }
            }
            else if (len == 2) {
                for (int i = 0; i < s.size() - 1; i++) {
                    dp[i][i + 1] = s[i] == s[i + 1];
                }
            }
            else {
                for (int i = 0; i <= s.size() - len; i++) {
                    dp[i][i + len - 1] = dp[i + 1][i + len - 2] && s[i] == s[i + len - 1];
                }
            }
        }
        for (int len = s.size(); len >= 1; len--) {
            for (int i = 0; i <= s.size() - len; i++) if (dp[i][i + len -1]) return s.substr(i, len);
        }
        return "";
    }
};
```

#### 90. 97. Interleaving String

Given strings s1, s2, and s3, find whether s3 is formed by an interleaving of s1 and s2.

An interleaving of two strings s and t is a configuration where s and t are divided into n and m 
substrings
 respectively, such that:

s = s1 + s2 + ... + sn
t = t1 + t2 + ... + tm
|n - m| <= 1
The interleaving is s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...
Note: a + b is the concatenation of strings a and b.
 

Example 1:


Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
Explanation: One way to obtain s3 is:
Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a".
Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac".
Since s3 can be obtained by interleaving s1 and s2, we return true.
Example 2:

Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
Explanation: Notice how it is impossible to interleave s2 with any other string to obtain s3.
Example 3:

Input: s1 = "", s2 = "", s3 = ""
Output: true

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        // dp[i][j] represents whether the first i characters 
        // of s1 and the first j characters of s2 can form the first i + j characters of s3
        int m = s1.size(), n = s2.size();
        if (m + n != s3.size()) return false;

        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        dp[0][0] = true;

        for (int i = 1; i <= m; ++i) dp[i][0] = dp[i - 1][0] && s1[i - 1] == s3[i - 1];

        for (int j = 1; j <= n; ++j) dp[0][j] = dp[0][j - 1] && s2[j - 1] == s3[j - 1];

        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                dp[i][j] = (dp[i - 1][j] && s1[i - 1] == s3[i + j - 1]) ||
                           (dp[i][j - 1] && s2[j - 1] == s3[i + j - 1]);
            }
        }

        return dp[m][n];
    }
};
```


#### 91. 72. Edit Distance

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character
Delete a character
Replace a character
 

Example 1:

Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
Example 2:

Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        const int m = word1.length();
        const int n = word2.length();
        // dp[i][j] := min # of operations to convert word1[0..i) to word2[0..j)
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));

        for (int i = 1; i <= m; ++i)
            dp[i][0] = i;

        for (int j = 1; j <= n; ++j)
            dp[0][j] = j;

        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
                if (word1[i - 1] == word2[j - 1]) // same characters
                    dp[i][j] = dp[i - 1][j - 1];  // no operation
                else
                    dp[i][j] =
                        min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
                              // replace       //delete        //insert
        return dp[m][n];
    }
};
```

#### 92. 141. Linked List Cycle

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.
 

Example 1:


Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
Example 2:


Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
Example 3:


Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* slow = head, *fast = head;
        while (fast != nullptr && fast->next != nullptr && fast->next->next != nullptr) {
            if (slow == fast) return true;
            slow = slow->next;
            fast = fast->next;
        }
        return false;
    }
};
```

#### 93. 200. Number of Islands

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.


Example 1:

Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
Example 2:

Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3

```cpp
class Solution {
public:
    void dfs(int r, int c, vector<vector<char>>& grid) {
        if (r < 0 || c < 0 || r == grid.size() || c == grid[0].size()) return;
        if (grid[r][c] == '0' || grid[r][c] == '2') return;
        grid[r][c] = '2';
        vector<vector<int>> dir = { {0, 1}, {1, 0}, {-1, 0}, {0, -1} };
        for (const auto& d: dir) dfs(r + d[0], c + d[1], grid);
    }

    int numIslands(vector<vector<char>>& grid) {
        int res = 0;
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (grid[i][j] == '1') {
                    res++;
                    dfs(i, j, grid);
                }
            }
        }
        return res;
    }
};
```

#### 94. 130. Surrounded Regions

You are given an m x n matrix board containing letters 'X' and 'O', capture regions that are surrounded:

Connect: A cell is connected to adjacent cells horizontally or vertically.
Region: To form a region connect every 'O' cell.
Surround: The region is surrounded with 'X' cells if you can connect the region with 'X' cells and none of the region cells are on the edge of the board.
To capture a surrounded region, replace all 'O's with 'X's in-place within the original board. You do not need to return anything.
 

Example 1:

Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

Explanation:


In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

Example 2:

Input: board = [["X"]]

Output: [["X"]]

```cpp

class Solution {
public:

    void bfs(queue<vector<int>> q, vector<vector<char>> &arr){
        vector<vector<int>> dir = {{0,1},{0,-1},{-1,0},{1,0}};
        int n = arr.size();
        int m = arr[0].size();

        while(q.size()!=0){
            vector<int> a = q.front();
            q.pop();
            int i = a[0];
            int j = a[1];
            arr[i][j]='Y';

            for(int k=0; k<4; k++){
                int nr = i+dir[k][0];
                int nc = j+dir[k][1];

                if(nr>=0 && nc>=0 && nr<n && nc<m && arr[nr][nc]=='O'){
                    arr[nr][nc]='Y';
                    q.push({nr,nc});
                }
            }
        }
    }
    void solve(vector<vector<char>>& arr) {
        int n = arr.size();
        int m = arr[0].size();
        queue<vector<int>> q;
        for(int i=0; i<n; i++){
            if(arr[i][0]=='O'){
                q.push({i,0});
            }
            if(arr[i][m-1]=='O'){
                q.push({i,m-1});
            }
        }
        for(int j=1; j<m-1; j++){
            if(arr[0][j]=='O') q.push({0,j});
            if(arr[n-1][j]=='O') q.push({n-1,j});
        }
        bfs(q,arr);
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(arr[i][j]=='Y') arr[i][j]='O';
                else if(arr[i][j]=='O') arr[i][j] = 'X';
            }
        }
    }
};
```


#### 95. 133. Clone Graph

Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

class Node {
    public int val;
    public List<Node> neighbors;
}


Test case format:

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with val == 1, the second node with val == 2, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.
 

Example 1:


Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
Example 2:


Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
Example 3:

Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
public:
    Node* dfs(Node* node, unordered_map<Node*, Node*>& vis) {
        if (node == nullptr) return nullptr;
        if (vis.find(node) != vis.end()) return vis[node];

        Node* root = new Node(node->val);
        vis[node] = root;
        for (Node* neighbor: node->neighbors) {
            root->neighbors.push_back(dfs(neighbor, vis));
        }
        return root;
    }

    Node* cloneGraph(Node* node) {
        unordered_map<Node*, Node*> vis;
        return dfs(node, vis);
    }
};
```

#### 96. 207. Course Schedule

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.
 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

```cpp
class Solution {
public:
    bool cycle(int u, const vector<vector<int>>& gr, vector<bool>& vis, unordered_set<int>& curr) {
        curr.insert(u);
        vis[u] = true;
        for (const auto& v: gr[u]) {
            if (curr.find(v) != curr.end()) return true;
            if (!vis[v] && cycle(v, gr, vis, curr)) return true;
        }
        curr.erase(u);
        return false;
    }

    bool canFinish(int n, vector<vector<int>>& pre) {
        vector<vector<int>> gr(n);
        for (const auto& p: pre) gr[p[1]].push_back(p[0]);
        vector<bool> vis(n);
        unordered_set<int> curr;
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                if (cycle(i, gr, vis, curr)) return false;
            }
        }
        return true;
    }
};
```

#### 97. 210. Course Schedule II

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.
 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
Example 2:

Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
Example 3:

Input: numCourses = 1, prerequisites = []
Output: [0]

```cpp
class Solution {
public:
    bool cycle(int u, const vector<vector<int>>& gr, vector<bool>& vis, unordered_set<int>& curr) {
        curr.insert(u);
        vis[u] = true;
        for (const auto& v: gr[u]) {
            if (curr.find(v) != curr.end()) return true;
            if (!vis[v] && cycle(v, gr, vis, curr)) return true;
        }
        curr.erase(u);
        return false;
    }

    void dfs(int u, const vector<vector<int>>& gr, vector<bool>& vis, vector<int>& st) {
        vis[u] = true;
        for (const auto& v: gr[u]) if (!vis[v]) dfs(v, gr, vis, st);
        st.push_back(u);
    }

    vector<int> findOrder(int n, vector<vector<int>>& pre) {
        vector<vector<int>> gr(n);
        for (const auto& p: pre) gr[p[1]].push_back(p[0]);
        vector<bool> vis(n);
        unordered_set<int> curr;
        bool contains = false;
        for (int i = 0; i < n; i++) {
            if (!vis[i]) contains |= cycle(i, gr, vis, curr);
        }
        if (contains) return {};

        vis = vector<bool>(n);
        vector<int> st;
        for (int i = 0; i < n; i++) if (!vis[i]) dfs(i, gr, vis, st);
        reverse(st.begin(), st.end());
        return st;
    }
};
```

#### 98. 399. Evaluate Division

You are given an array of variable pairs equations and an array of real numbers values, where equations[i] = [Ai, Bi] and values[i] represent the equation Ai / Bi = values[i]. Each Ai or Bi is a string that represents a single variable.

You are also given some queries, where queries[j] = [Cj, Dj] represents the jth query where you must find the answer for Cj / Dj = ?.

Return the answers to all queries. If a single answer cannot be determined, return -1.0.

Note: The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

Note: The variables that do not occur in the list of equations are undefined, so the answer cannot be determined for them.
 

Example 1:

Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation: 
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? 
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
note: x is undefined => -1.0
Example 2:

Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]
Example 3:

Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]

```cpp
class Solution {
public:

    void dfs(string node, string& dest, unordered_map<string, unordered_map<string, double>>& gr, unordered_set<string>& vis, double& ans, double temp) {
        if(vis.find(node) != vis.end()) return;

        vis.insert(node);
        if(node == dest){
            ans = temp;
            return;
        }

        for(auto ne : gr[node]){
            dfs(ne.first, dest, gr, vis, ans, temp * ne.second);
        }
    }

    unordered_map<string, unordered_map<string, double>> buildGraph(const vector<vector<string>>& equations, const vector<double>& values) {
        unordered_map<string, unordered_map<string, double>> gr;

        for (int i = 0; i < equations.size(); i++) {
            string dividend = equations[i][0];
            string divisor = equations[i][1];
            double value = values[i];

            gr[dividend][divisor] = value;
            gr[divisor][dividend] = 1.0 / value;
        }

        return gr;
    }

    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_map<string, unordered_map<string, double>> gr = buildGraph(equations, values);
        vector<double> finalAns;

        for (auto query : queries) {
            string dividend = query[0];
            string divisor = query[1];

            if (gr.find(dividend) == gr.end() || gr.find(divisor) == gr.end()) {
                finalAns.push_back(-1.0);
            } else {
                unordered_set<string> vis;
                double ans = -1, temp=1.0;
                dfs(dividend, divisor, gr, vis, ans, temp);
                finalAns.push_back(ans);
            }
        }

        return finalAns;
    }
};
```


#### 99. 104. Maximum Depth of Binary Tree

Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
 

Example 1:

Input: root = [3,9,20,null,null,15,7]
Output: 3
Example 2:

Input: root = [1,null,2]
Output: 2

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```

#### 100. 100. Same Tree

Given the roots of two binary trees p and q, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.
 

Example 1:


Input: p = [1,2,3], q = [1,2,3]
Output: true
Example 2:


Input: p = [1,2], q = [1,null,2]
Output: false
Example 3:


Input: p = [1,2,1], q = [1,1,2]
Output: false

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == nullptr && q == nullptr) return true;
        if (p == nullptr || q == nullptr) return false;
        return p->val == q->val && isSameTree(p->left, q->left) && isSameTree(p->right, q->right); 
    }
};
```

#### 101. 226. Invert Binary Tree

Given the root of a binary tree, invert the tree, and return its root.
 

Example 1:


Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
Example 2:


Input: root = [2,1,3]
Output: [2,3,1]
Example 3:

Input: root = []
Output: []

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return root;
        auto l = invertTree(root->left);
        auto r = invertTree(root->right);
        root->left = r;
        root->right = l;
        return root;
    }
};
```

#### 102. 101. Symmetric Tree

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).


Example 1:


Input: root = [1,2,2,3,4,4,3]
Output: true
Example 2:


Input: root = [1,2,2,null,3,null,3]
Output: false

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool symmetric(TreeNode* left, TreeNode* right) {
        if (left == nullptr && right == nullptr) return true;
        if (left == nullptr || right == nullptr) return false;
        return left->val == right->val && 
               symmetric(left->left, right->right) && 
               symmetric(left->right, right->left);
    }

    bool isSymmetric(TreeNode* root) {
        return symmetric(root, root);
    }
};
```

#### 103. 105. Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.
 

Example 1:


Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
Example 2:

Input: preorder = [-1], inorder = [-1]
Output: [-1]

```cpp
class Solution {
public:
    TreeNode* constructTree(vector<int>& preorder, int preStart, int preEnd,
                            vector<int>& inorder, int inStart, int inEnd,
                            map<int, int>& mp) {
        if (preStart > preEnd || inStart > inEnd)
            return NULL;

        TreeNode* root = new TreeNode(preorder[preStart]);
        int elem = mp[root->val];
        int nElem = elem - inStart;

        root->left = constructTree(preorder, preStart + 1, preStart + nElem,
                                   inorder, inStart, elem - 1, mp);
        root->right = constructTree(preorder, preStart + nElem + 1, preEnd,
                                    inorder, elem + 1, inEnd, mp);

        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int preStart = 0, preEnd = preorder.size() - 1;
        int inStart = 0, inEnd = inorder.size() - 1;

        map<int, int> mp;
        for (int i = inStart; i <= inEnd; i++) {
            mp[inorder[i]] = i;
        }

        return constructTree(preorder, preStart, preEnd, inorder, inStart, inEnd, mp);
    }
};
```

#### 104. 106. Construct Binary Tree from Inorder and Postorder Traversal

Given two integer arrays inorder and postorder where inorder is the inorder traversal of a binary tree and postorder is the postorder traversal of the same tree, construct and return the binary tree.
 

Example 1:


Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
Example 2:

Input: inorder = [-1], postorder = [-1]
Output: [-1]

```cpp
class Solution {
public:
    TreeNode *Tree(vector<int>& in, int x, int y,vector<int>& po,int a,int b){
        if(x > y || a > b)return nullptr;
        TreeNode *node = new TreeNode(po[b]);
        int SI = x;  
        while(node->val != in[SI])SI++;
        node->left  = Tree(in, x, SI - 1, po, a, a + SI - x - 1);
        node->right = Tree(in, SI + 1, y, po, a + SI - x, b - 1);
        return node;
    }
    TreeNode* buildTree(vector<int>& in, vector<int>& po){
        return Tree(in, 0, in.size() - 1, po, 0, po.size() - 1);
    }
};
```

#### 105. 117. Populating Next Right Pointers in Each Node II

Given a binary tree

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.
 

Example 1:


Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
Example 2:

Input: root = []
Output: []

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (root == nullptr) return nullptr;
        queue<Node*> q;
        q.push(root);
        while (!q.empty()) {
            int sz = q.size();
            for (int i = 0; i < sz; i++) {
                Node* node = q.front();
                q.pop();
                if (i < sz - 1) node->next = q.front();
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
        }
        return root;
    }
};
```

#### 106. 114. Flatten Binary Tree to Linked List

Given the root of a binary tree, flatten the tree into a "linked list":

The "linked list" should use the same TreeNode class where the right child pointer points to the next node in the list and the left child pointer is always null.
The "linked list" should be in the same order as a pre-order traversal of the binary tree.
 

Example 1:

Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
Example 2:

Input: root = []
Output: []
Example 3:

Input: root = [0]
Output: [0]

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:

    pair<TreeNode*, TreeNode*> dfs(TreeNode* root) {
        if (root == nullptr) return {nullptr, nullptr};
        auto [sl, el] = dfs(root->left);
        auto [sr, er] = dfs(root->right);
        if (sl == nullptr && sr == nullptr) return {root, root};
        if (sl) {
            root->right = sl;
            if (sr) el->right = sr; 
        }
        root->left = nullptr;
        return {root, sr ? er : el};
    }

    void flatten(TreeNode* root) {
        dfs(root);
    }
};
```

#### 107. 112. Path Sum

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

A leaf is a node with no children.
 

Example 1:


Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
Explanation: The root-to-leaf path with the target sum is shown.
Example 2:


Input: root = [1,2,3], targetSum = 5
Output: false
Explanation: There are two root-to-leaf paths in the tree:
(1 --> 2): The sum is 3.
(1 --> 3): The sum is 4.
There is no root-to-leaf path with sum = 5.
Example 3:

Input: root = [], targetSum = 0
Output: false
Explanation: Since the tree is empty, there are no root-to-leaf paths.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum, int sum = 0) {
        if (root == nullptr) return false;
        if (root->left == nullptr && root->right == nullptr) return sum + root->val == targetSum;
        if (root->left == nullptr) return hasPathSum(root->right, targetSum, sum + root->val);
        if (root->right == nullptr) return hasPathSum(root->left, targetSum, sum + root->val);
        return hasPathSum(root->left, targetSum, sum + root->val) || 
               hasPathSum(root->right, targetSum, sum + root->val);
    }
};
```

#### 108. 129. Sum Root to Leaf Numbers

You are given the root of a binary tree containing digits from 0 to 9 only.

Each root-to-leaf path in the tree represents a number.

For example, the root-to-leaf path 1 -> 2 -> 3 represents the number 123.
Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.

A leaf node is a node with no children.


Example 1:


Input: root = [1,2,3]
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
Example 2:


Input: root = [4,9,0,5,1]
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, int curr, int& sum) {
        if (root->left == nullptr && root->right == nullptr) {
            sum += curr * 10 + root->val;
            return;
        }
        if (root->left != nullptr) dfs(root->left, curr * 10 + root->val, sum);
        if (root->right != nullptr) dfs(root->right, curr * 10 + root->val, sum);
    }

    int sumNumbers(TreeNode* root) {
        int curr = 0, sum = 0;
        dfs(root, curr, sum);
        return sum;
    }
};
```

#### 109. 124. Binary Tree Maximum Path Sum

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.
 

Example 1:


Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
Example 2:


Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:

    int f(TreeNode* root, int& res) {
        if (root == nullptr) return 0;
        int l = max(0, f(root->left, res));
        int r = max(0, f(root->right, res));
        res = max(res, root->val + l + r);
        return root->val + max(l, r);
    }

    int maxPathSum(TreeNode* root) {
        int res = -1e9;
        f(root, res);
        return res;
    }
};
```

#### 110. 173. Binary Search Tree Iterator

Implement the BSTIterator class that represents an iterator over the in-order traversal of a binary search tree (BST):

BSTIterator(TreeNode root) Initializes an object of the BSTIterator class. The root of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
boolean hasNext() Returns true if there exists a number in the traversal to the right of the pointer, otherwise returns false.
int next() Moves the pointer to the right, then returns the number at the pointer.
Notice that by initializing the pointer to a non-existent smallest number, the first call to next() will return the smallest element in the BST.

You may assume that next() calls will always be valid. That is, there will be at least a next number in the in-order traversal when next() is called.

 

Example 1:


Input
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
Output
[null, 3, 7, true, 9, true, 15, true, 20, false]

Explanation
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // return 3
bSTIterator.next();    // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 20
bSTIterator.hasNext(); // return False


```cpp
class BSTIterator {
    stack<TreeNode *> myStack;
public:
    BSTIterator(TreeNode *root) {
        pushAll(root);
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !myStack.empty();
    }

    /** @return the next smallest number */
    int next() {
        TreeNode *tmpNode = myStack.top();
        myStack.pop();
        pushAll(tmpNode->right);
        return tmpNode->val;
    }

private:
    void pushAll(TreeNode *node) {
        for (; node != NULL; myStack.push(node), node = node->left);
    }
};
```

#### 111. 222. Count Complete Tree Nodes

Given the root of a complete binary tree, return the number of the nodes in the tree.

According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

Design an algorithm that runs in less than O(n) time complexity.
 

Example 1:


Input: root = [1,2,3,4,5,6]
Output: 6
Example 2:

Input: root = []
Output: 0
Example 3:

Input: root = [1]
Output: 1

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

#### 112. 236. Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”
 

Example 1:


Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
Example 2:


Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
Example 3:

Input: root = [1,2], p = 1, q = 2
Output: 1

```cpp
if(root == null || root.val == p.val || root.val == q.val){
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        return ((left != null) ? ((right != null) ? root : left) : right);
```

#### 113. 530. Minimum Absolute Difference in BST

Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.
 

Example 1:


Input: root = [4,2,6,1,3]
Output: 1
Example 2:


Input: root = [1,0,48,null,null,12,49]
Output: 1 

```cpp
class Solution {
public:
    int diff = INT_MAX;
    TreeNode *prev = NULL;
    void dfs(TreeNode *root) {
        if (root->left) dfs(root->left);
        if (prev) diff = min(diff, abs(prev->val - root->val));
        prev = root;
        if (root->right) dfs(root->right);
    }
    int getMinimumDifference(TreeNode *root) {
        dfs(root);
        return diff;
    }
};
```

#### 114. 230. Kth Smallest Element in a BST

Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.
 

Example 1:


Input: root = [3,1,4,null,2], k = 1
Output: 1
Example 2:


Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3 

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:

    int dfs(TreeNode* root, int& k) {
        if (root == nullptr) return -1;
        int left = dfs(root->left, k);
        if (left != -1) return left; 
        k--;
        if (k == 0) return root->val; 
        return dfs(root->right, k);  
    }

    int kthSmallest(TreeNode* root, int k) {
        return dfs(root, k);
    }
};
```

#### 115. 98. Validate Binary Search Tree

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left 
subtree
 of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:

Input: root = [2,1,3]
Output: true
Example 2:


Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    pair<long long, long long> dfs(TreeNode* root, bool& res) {
        if (root == nullptr) return {1e18, -1e18};
        auto [mnl, mxl] = dfs(root->left, res);
        auto [mnr, mxr] = dfs(root->right, res);
        if (root->val <= mxl || root->val >= mnr) res = false;
        return {min({mnl, mnr, (long long)root->val}), 
                max({mxl, mxr, (long long)root->val})};
    }

    bool isValidBST(TreeNode* root) {
        bool res = true;
        dfs(root, res);
        return res;
    }
};
```

#### 116. 199. Binary Tree Right Side View

Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

 

Example 1:

Input: root = [1,2,3,null,5,null,4]

Output: [1,3,4]

Explanation:



Example 2:

Input: root = [1,2,3,4,null,null,null,5]

Output: [1,3,4,5]

Explanation:


Example 3:

Input: root = [1,null,3]

Output: [1,3]

Example 4:

Input: root = []

Output: []

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, int curr, int& next, vector<int>& res) {
        if (root == nullptr) return;
        if (curr == next) {
            res.push_back(root->val);
            next++;
        }
        dfs(root->right, curr + 1, next, res);
        dfs(root->left, curr + 1, next, res);
    }

    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        int next = 0;
        dfs(root, 0, next, res);
        return res;
    }
};
```

#### 117. 637. Average of Levels in Binary Tree

Given the root of a binary tree, return the average value of the nodes on each level in the form of an array. Answers within 10-5 of the actual answer will be accepted. 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: [3.00000,14.50000,11.00000]
Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.
Hence return [3, 14.5, 11].
Example 2:


Input: root = [3,9,20,15,7]
Output: [3.00000,14.50000,11.00000]

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, int level, vector<pair<long long, int>>& avg) {
        if (root == nullptr) return;
        if (avg.size() == level) avg.push_back({root->val, 1});
        else avg[level].first += root->val, avg[level].second++;
        dfs(root->left, level + 1, avg);
        dfs(root->right, level + 1, avg);
    }

    vector<double> averageOfLevels(TreeNode* root) {
        vector<pair<long long, int>> avg;
        dfs(root, 0, avg);
        vector<double> res;
        for (const auto& [s, n]: avg) res.push_back((double)s / (double)n);
        return res;        
    }
};
```

#### 118. 102. Binary Tree Level Order Traversal

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).
 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
Example 2:

Input: root = [1]
Output: [[1]]
Example 3:

Input: root = []
Output: []

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, int level, vector<vector<int>>& res) {
        if (root == nullptr) return;
        if (res.size() == level) res.push_back({root->val});
        else res[level].push_back(root->val);
        dfs(root->left, level + 1, res);
        dfs(root->right, level + 1, res);
    }

    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        dfs(root, 0, res);
        return res;
    }
};
```

#### 119. 103. Binary Tree Zigzag Level Order Traversal

Given the root of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).
 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
Example 2:

Input: root = [1]
Output: [[1]]
Example 3:

Input: root = []
Output: []

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (root == nullptr) return {};
        vector<vector<int>> res;
        queue<TreeNode*> q;
        q.push(root);
        int level = 0;
        while (!q.empty()) {
            int sz = q.size();
            vector<int> r;
            while (sz--) {
                auto node = q.front();
                r.push_back(node->val);
                q.pop();
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            if (level&1) reverse(r.begin(), r.end());
            res.push_back(r);
            level++;
        }
        return res;
    }
};
```

#### 120. 9. Palindrome Number

Given an integer x, return true if x is a 
palindrome, and false otherwise. 

Example 1:

Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
Example 2:

Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
Example 3:

Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return false;
        long long xx = x, y = 0;
        while (xx) {
            y = y * 10 + (xx % 10);
            xx /= 10;
        }
        return x == y;
    }
};
```

#### 121. 66. Plus One

You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.
 

Example 1:

Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].
Example 2:

Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].
Example 3:

Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        reverse(digits.begin(), digits.end());
        int carry = 1;
        for (auto& digit: digits) {
            int curr = digit + carry;
            digit = curr % 10;
            carry = curr / 10;
        }
        if (carry) digits.push_back(carry);
        reverse(digits.begin(), digits.end());
        return digits;
    }
};
```

#### 122. 172. Factorial Trailing Zeroes

Given an integer n, return the number of trailing zeroes in n!.

Note that n! = n * (n - 1) * (n - 2) * ... * 3 * 2 * 1.
 

Example 1:

Input: n = 3
Output: 0
Explanation: 3! = 6, no trailing zero.
Example 2:

Input: n = 5
Output: 1
Explanation: 5! = 120, one trailing zero.
Example 3:

Input: n = 0
Output: 0

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        return n == 0 ? 0 : n / 5 + trailingZeroes(n / 5);
    }
};
```

#### 123. 69. Sqrt(x)

Given a non-negative integer x, return the square root of x rounded down to the nearest integer. The returned integer should be non-negative as well.

You must not use any built-in exponent function or operator.

For example, do not use pow(x, 0.5) in c++ or x ** 0.5 in python.
 

Example 1:

Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
Example 2:

Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int start = 0, end = x;
        int ans = 0;
        long mid;
        while (start <= end) {
            long mid = start + (end - start) / 2;
            long mul = mid * mid;
            if (mul == x) {
                return mid;
            } 
            else if (mul > x) {
                end = mid - 1;
            } 
            else {
                start = mid + 1;
                ans = mid;
            }
        }
        return ans;
    }
};
```

#### 124. 50. Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (i.e., xn).
 

Example 1:

Input: x = 2.00000, n = 10
Output: 1024.00000
Example 2:

Input: x = 2.10000, n = 3
Output: 9.26100
Example 3:

Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        return binaryExp(x, static_cast<long>(n));
    }

private:
    double binaryExp(double x, long n) {
        if (n == 0) {
            return 1;
        }
       
        if (n < 0) {
            return 1.0 / binaryExp(x, -n);
        }
        if (n % 2 == 1) {
            return x * binaryExp(x * x, (n - 1) / 2);
        } 
        else {
            return binaryExp(x * x, n / 2);
        }
    }
};
```

#### 125. 149. Max Points on a Line

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane, return the maximum number of points that lie on the same straight line.


Example 1:


Input: points = [[1,1],[2,2],[3,3]]
Output: 3
Example 2:


Input: points = [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4

```cpp
class Solution {
public:
    int maxPoints(vector<vector<int>>& pt) {
        int ans = 1; 
        int n = pt.size();
        for(int i=0; i < n - 1; i++){
            map<double,int> mp;
            for(int j = i + 1; j < n; j++){
                double x = (double)(pt[j][1] - pt[i][1]) / (double)(pt[j][0] - pt[i][0]);
                if(pt[j][1] - pt[i][1] < 0 && (pt[j][0] - pt[i][0]) == 0) mp[abs(x)]++; 
                else mp[x]++;
            }
            int temp = 0;
            for(auto it:mp) temp = max(temp, it.second+1);
            ans = max(temp, ans);
        }
        return ans;
    }
};
```

#### 126. 67. Add Binary

Given two binary strings a and b, return their sum as a binary string.
 

Example 1:

Input: a = "11", b = "1"
Output: "100"
Example 2:

Input: a = "1010", b = "1011"
Output: "10101"

```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        reverse(a.begin(), a.end());
        reverse(b.begin(), b.end());

        string res;
        int carry = 0;
        int n = max(a.size(), b.size());

        for (int i = 0; i < n; i++) {
            int digitA = (i < a.size()) ? (a[i] - '0') : 0;
            int digitB = (i < b.size()) ? (b[i] - '0') : 0;
            int curr = digitA + digitB + carry;

            res.push_back('0' + (curr % 2)); 
            carry = curr / 2; 
        }

        if (carry) res.push_back('1'); 

        reverse(res.begin(), res.end()); 
        return res;
    }
};
```

#### 127. 190. Reverse Bits

Reverse bits of a given 32 bits unsigned integer.

Note:

Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above, the input represents the signed integer -3 and the output represents the signed integer -1073741825.
 

Example 1:

Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
Example 2:

Input: n = 11111111111111111111111111111101
Output:   3221225471 (10111111111111111111111111111111)
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111

```cpp
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        for (int i = 0, j = 31; i < j; i++, j--) {
            int f = n & (1 << i), s = n & (1 << j);
            if (s) n = n | (1 << i);
            else n = n & ~(1 << i);
            if (f) n = n | (1 << j);
            else n = n & ~(1 << j);
        }
        return n;
    }
};
```

#### 128. Given a positive integer n, write a function that returns the number of 
set bits in its binary representation (also known as the Hamming weight).
 

Example 1:

Input: n = 11

Output: 3

Explanation:

The input binary string 1011 has a total of three set bits.

Example 2:

Input: n = 128

Output: 1

Explanation:

The input binary string 10000000 has a total of one set bit.

Example 3:

Input: n = 2147483645

Output: 30

Explanation:

The input binary string 1111111111111111111111111111101 has a total of thirty set bits

```cpp
class Solution {
public:
    int hammingWeight(int n) {
        int res = 0;
        while (n) {
            res += n & 1;
            n >>= 1;
        }
        return res;
    }
};
```

#### 129. 136. Single Number

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.


Example 1:

Input: nums = [2,2,1]

Output: 1

Example 2:

Input: nums = [4,1,2,1,2]

Output: 4

Example 3:

Input: nums = [1]

Output: 1

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = nums.back();
        nums.pop_back();
        for (const auto& num: nums) res ^= num;
        return res;
    }
};
```

#### 130. 137. Single Number II

Given an integer array nums where every element appears three times except for one, which appears exactly once. Find the single element and return it.

You must implement a solution with a linear runtime complexity and use only constant extra space.
 

Example 1:

Input: nums = [2,2,3,2]
Output: 3
Example 2:

Input: nums = [0,1,0,1,0,1,99]
Output: 99

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result = 0; 
        for (int i = 0; i < 32; i++) {
            int count = 0;
            for (int num : nums) {
                if (num & (1 << i)) count++;
            }
            if (count % 3 != 0) {
                result |= (1 << i);
            }
        }
        return result;
    }
};
```

#### 131. 201. Bitwise AND of Numbers Range

Given two integers left and right that represent the range [left, right], return the bitwise AND of all numbers in this range, inclusive.
 

Example 1:

Input: left = 5, right = 7
Output: 4
Example 2:

Input: left = 0, right = 0
Output: 0
Example 3:

Input: left = 1, right = 2147483647
Output: 0

```cpp

class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        int cnt = 0;
        while (left != right) {
            left >>= 1;
            right >>= 1;
            cnt++;
        }
        return (left << cnt);
    }
};
```

#### 132. 208. Implement Trie (Prefix Tree)

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

Trie() Initializes the trie object.
void insert(String word) Inserts the string word into the trie.
boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.
 

Example 1:

Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True

```cpp
struct Node {
    vector<Node*> child;
    bool last;
    Node() : child(vector<Node*>(26)), last(false) {}
};

class Trie {
private: 
    Node* root;
public:
    Trie() : root(new Node()) {
        
    }
    
    void insert(string word) {
        insert(root, word, 0);
    }

    void insert(Node* node, const string& word, int idx) {
        if (idx == word.size() - 1) {
            if (node->child[word[idx] - 'a'] == nullptr) node->child[word[idx] - 'a'] = new Node();
            node->child[word[idx] - 'a']->last = true;
            return;
        }
        if (node->child[word[idx] - 'a'] == nullptr) node->child[word[idx] - 'a'] = new Node();
        insert(node->child[word[idx] - 'a'], word, idx + 1);
    }
    
    bool search(string word) {
        return search(root, word, 0);
    }

    bool search(Node* node, const string& word, int idx) {
        if (idx == word.size() - 1) {
            return node->child[word[idx] - 'a'] != nullptr && node->child[word[idx] - 'a']->last;
        }
        if (node->child[word[idx] - 'a'] == nullptr) return false;
        return search(node->child[word[idx] - 'a'], word, idx + 1);
    }
    
    bool startsWith(string prefix) {
        return startsWith(root, prefix, 0);
    }

    bool startsWith(Node* node, const string& word, int idx) {
        if (idx == word.size()) return true;
        if (node->child[word[idx] - 'a'] == nullptr) return false;
        return startsWith(node->child[word[idx] - 'a'], word, idx + 1);
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

#### 133. 211. Design Add and Search Words Data Structure

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the WordDictionary class:

WordDictionary() Initializes the object.
void addWord(word) Adds word to the data structure, it can be matched later.
bool search(word) Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter.
 

Example:

Input
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
Output
[null,null,null,null,false,true,true,true]

Explanation
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True

```cpp
struct Node {
    vector<Node*> child;
    bool last;
    Node() : child(vector<Node*>(26)), last(false) {}
};

class WordDictionary {
private: 
    Node* root;

public:
    WordDictionary() : root(new Node()) {
        
    }
    
    void addWord(string word) {
        addWord(root, word, 0);
    }

    void addWord(Node* node, const string& word, int idx) {
        if (idx == word.size() - 1) {
            if (node->child[word[idx] - 'a'] == nullptr) node->child[word[idx] - 'a'] = new Node();
            node->child[word[idx] - 'a']->last = true;
            return;
        }
        if (node->child[word[idx] - 'a'] == nullptr) node->child[word[idx] - 'a'] = new Node();
        addWord(node->child[word[idx] - 'a'], word, idx + 1);
    }
    
    bool search(string word) {
        return search(root, word, 0);
    }

    bool search(Node* node, const string& word, int idx) {
        if (idx == word.size() - 1) {
            if (word[idx] == '.') {
                for (const auto& c: node->child) if (c != nullptr && c->last) return true;
                return false; 
            }
            return node->child[word[idx] - 'a'] != nullptr && node->child[word[idx] - 'a']->last;
        }
        if (word[idx] == '.') {
            bool res = false;
            for (const auto& c: node->child) if (c != nullptr) res |= search(c, word, idx + 1);
            return res;
        }
        if (node->child[word[idx] - 'a'] == nullptr) return false;
        return search(node->child[word[idx] - 'a'], word, idx + 1);
    }
};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```

#### 134. 212. Word Search II

Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
 

Example 1:


Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
Example 2:


Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []

```cpp
struct Node {
    vector<Node*> next;
    vector<int> cnt;

    Node(): next(27, nullptr), cnt(27, 0) {}
};

class Solution {
public:
    vector<string> result;
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        Node* root = new Node();
        for (string word : words) {
            insert(word, root);
        }

        for(int i = 0 ; i < board.size() ; ++i)
            for(int j = 0 ; j < board[0].size() ; ++j)
                helper(board, i, j, "", root);

        return result;
    }

    void helper(vector<vector<char>>& board, int i, int j, string pre, Node* root) {

        if (i < 0 || i >= board.size() || j < 0 || j >= board[0].size() || board[i][j] == '*')
            return;

        pre += board[i][j];
        pair<bool,bool> f = find(pre, root);

        if(!f.second) 
            return;
        
        if(f.first) {
            result.push_back(pre);
            deleteWord(pre, root);
        }
        
        // cout << pre << ' ' << i << ' ' << j << ' ' << f.first << ' ' << f.second << endl;

        char c = board[i][j];
        board[i][j] = '*';

        helper(board, i + 1, j, pre, root);
        helper(board, i, j + 1, pre, root);
        helper(board, i - 1, j, pre, root);
        helper(board, i, j - 1, pre, root);

        board[i][j] = c;
    }



    void insert(string word, Node* root) {
        for(int i = 0 ; i < word.size() ; ++i) {
            if(!root->next[word[i]-'a']) {            
                root->next[word[i]-'a'] = new Node();
            }
            
            root->cnt[word[i]-'a']++;
            root = root->next[word[i]-'a'];
        }
        if (!root->next[26]) {           
            root->next[26] = new Node();
        }
        root->cnt[26]++;
    }

    pair<bool,bool> find(string word, Node* root) {
        for(int i = 0 ; i < word.size() ; ++i) {
            if(!root->next[word[i]-'a']) {            
                return {false, false};           
            }

            root = root->next[word[i]-'a'];
        }

        return {root->next[26], true};
    }

    void deleteWord(string word, Node* root) {
        for(char c : word) {
            root->cnt[c-'a']--;
            Node* nxt = root->next[c-'a'];
            if(root->cnt[c-'a'] == 0) {
                root->next[c-'a'] = NULL;
            }
            root = nxt;
        }
        root->cnt[26]--;
        if(root->cnt[26] == 0) {
            root->next[26] = NULL;
        }
    }
};
```

#### 135. 2. Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
 

Example 1:


Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
Example 2:

Input: l1 = [0], l2 = [0]
Output: [0]
Example 3:

Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummyHead = new ListNode(0);
        ListNode* tail = dummyHead;
        int carry = 0;

        while (l1 != nullptr || l2 != nullptr || carry != 0) {
            int digit1 = (l1 != nullptr) ? l1->val : 0;
            int digit2 = (l2 != nullptr) ? l2->val : 0;

            int sum = digit1 + digit2 + carry;
            int digit = sum % 10;
            carry = sum / 10;

            ListNode* newNode = new ListNode(digit);
            tail->next = newNode;
            tail = tail->next;

            l1 = (l1 != nullptr) ? l1->next : nullptr;
            l2 = (l2 != nullptr) ? l2->next : nullptr;
        }

        ListNode* result = dummyHead->next;
        delete dummyHead;
        return result;
    }
};
```

#### 136. 21. Merge Two Sorted Lists

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.
 

Example 1:


Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
Example 2:

Input: list1 = [], list2 = []
Output: []
Example 3:

Input: list1 = [], list2 = [0]
Output: [0]

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* res = nullptr, *curr = nullptr;
        while (list1 && list2) {
            if (list1->val <= list2->val) {
                ListNode* node = new ListNode(list1->val);
                if (res == nullptr) res = node;
                else curr->next = node;
                curr = node;
                list1 = list1->next;
            }
            else {
                ListNode* node = new ListNode(list2->val);
                if (res == nullptr) res = node;
                else curr->next = node;
                curr = node;
                list2 = list2->next;
            }
        }
        while (list1) {
            ListNode* node = new ListNode(list1->val);
            if (res == nullptr) res = node;
            else curr->next = node;
            curr = node;
            list1 = list1->next;
        }
        while (list2) {
            ListNode* node = new ListNode(list2->val);
            if (res == nullptr) res = node;
            else curr->next = node;
            curr = node;
            list2 = list2->next;
        }
        return res;
    }
};
```

#### 137. 138. Copy List with Random Pointer

A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null.

Construct a deep copy of the list. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

For example, if there are two nodes X and Y in the original list, where X.random --> Y, then for the corresponding two nodes x and y in the copied list, x.random --> y.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of n nodes. Each node is represented as a pair of [val, random_index] where:

val: an integer representing Node.val
random_index: the index of the node (range from 0 to n-1) that the random pointer points to, or null if it does not point to any node.
Your code will only be given the head of the original linked list.
 

Example 1:


Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
Example 2:


Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
Example 3:



Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]] 

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        map<Node*, Node*> oc, co;
        Node* res = nullptr, *curr = nullptr;
        while (head) {
            Node* node = new Node(head->val);
            oc[head] = node;
            co[node] = head;
            if (res == nullptr) res = node;
            else curr->next = node;
            curr = node;
            head = head->next;
        }
        curr = res;
        while (curr) {
            if (co[curr]->random != nullptr) curr->random = oc[co[curr]->random];
            curr = curr->next;
        }
        return res;
    }
};
```

#### 138. 92. Reverse Linked List II

Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.
 

Example 1:


Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
Example 2:

Input: head = [5], left = 1, right = 1
Output: [5]

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode *dummy = new ListNode(0);
        dummy->next = head;
        ListNode *prev = dummy;
        for(int i = 0; i < left - 1; i++) prev = prev->next;
        ListNode *curr = prev->next;
        for(int i = 0; i < right - left; i++){
            ListNode *forw = curr->next;
            curr->next = forw->next;
            forw->next = prev->next;
            prev->next = forw;
        }
        return dummy->next;
    }
};
```

#### 139. 61. Rotate List

Given the head of a linked list, rotate the list to the right by k places.
 

Example 1:


Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
Example 2:


Input: head = [0,1,2], k = 4
Output: [2,0,1]

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int len(ListNode* head) {
        int res = 0;
        while (head) {
            res++;
            head = head->next;
        }
        return res;
    }

    ListNode* rotateRight(ListNode* head, int k) {
        if (head == nullptr) return head;
        k %= len(head);
        if (k == 0) return head;
        ListNode* left = nullptr, *right = head;
        while (k-- > 1) right = right->next;
        while (right->next != nullptr) {
            left = left == nullptr ? head : left->next;
            right = right->next;
        }
        ListNode* res = left->next;
        left->next = nullptr;
        right->next = head;
        return res;
    }
};
```

#### 140. 19. Remove Nth Node From End of List

Given the head of a linked list, remove the nth node from the end of the list and return its head.

Example 1:


Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
Example 2:

Input: head = [1], n = 1
Output: []
Example 3:

Input: head = [1,2], n = 1
Output: [1]

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* left = nullptr, *right = head;
        while (n-- > 1) right = right->next;
        while (right->next) {
            left = left == nullptr ? head : left->next;
            right = right->next;
        }
        if (left == nullptr) return head->next;
        else left->next = left->next->next;
        return head;
    }
};
```

#### 141. 86. Partition List

Given the head of a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

Example 1:


Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
Example 2:

Input: head = [2,1], x = 2
Output: [1,2]

```cpp
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode *leftHead = new ListNode(0), *rightHead = new ListNode(0);
        ListNode *left = leftHead, *right = rightHead;
        while (head) {
            if (head->val < x) {
                left->next = head;
                left = left->next;
            } 
            else {
                right->next = head;
                right = right->next;
            }
            head = head->next;
        }
        right->next = nullptr; 
        left->next = rightHead->next;
        return leftHead->next;
    }
};
```

#### 142. 82. Remove Duplicates from Sorted List II

Given the head of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.
 

Example 1:

Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
Example 2:


Input: head = [1,1,1,2,3]
Output: [2,3]

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next) return head;
        int val = head->val;
        ListNode* p = head->next;
        if (p->val != val) {
            head->next = deleteDuplicates(p);
            return head;
        } 
        else {
            while (p && p->val == val) p = p->next;
            return deleteDuplicates(p);
        }
    }
};
```

#### 143. 25. Reverse Nodes in k-Group

Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.
 

Example 1:

Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
Example 2:


Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int count(ListNode* head){
        int n = 0;
        while(head){
            head=head->next;
            n++;
        }
        return n;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
          int num = count(head);                                      
          if(num < k){
              return head;
          }
          int x = k;
          ListNode *next, *prev=NULL;
          ListNode *curr = head;
          while(x--){
              next = curr->next;
              curr->next = prev;
              prev = curr;
              curr = next;
          }
          // 1->2->3->4....
          head->next = reverseKGroup(next,k);
          return prev;
    }
};
```

#### 144. 146. LRU Cache

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
int get(int key) Return the value of the key if the key exists, otherwise return -1.
void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
The functions get and put must each run in O(1) average time complexity.
 

Example 1:

Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4

```cpp
struct pair_hash {
    std::size_t operator()(const std::pair<int, int>& p) const {
        return std::hash<int>()(p.first) ^ std::hash<int>()(p.second);
    }
};

class LRUCache {
private:
    int curr;
    set<pair<int, pair<int, int>>> st;
    unordered_map<pair<int, int>, int, pair_hash> tst;
    int tot;
    unordered_map<int, int> mp;

public:
    LRUCache(int capacity) : curr(0), tot(capacity) {
        
    }
    
    int get(int key) {
        if (mp.find(key) != mp.end()) {
            st.erase({tst[{key, mp[key]}], {key, mp[key]}});
            st.insert({curr, {key, mp[key]}});
            tst[{key, mp[key]}] = curr;
            curr++;
            return mp[key];
        }
        return -1;
    }
    
    void put(int key, int value) {
        if (mp.find(key) != mp.end()) {
            st.erase({tst[{key, mp[key]}], {key, mp[key]}});
            st.insert({curr, {key, value}});
            tst[{key, value}] = curr;
            mp[key] = value;
            curr++;
        }
        else {
            if (mp.size() == tot) {
                int k = st.begin()->second.first, v = st.begin()->second.second;
                st.erase(st.begin());
                tst.erase({k, v});
                mp.erase(k);
            }
            st.insert({curr, {key, value}});
            tst[{key, value}] = curr;
            mp[key] = value;
            curr++;
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

#### 145. 221. Maximal Square

Given an m x n binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.
 

Example 1:

Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
Example 2:


Input: matrix = [["0","1"],["1","0"]]
Output: 1
Example 3:

Input: matrix = [["0"]]
Output: 0

```cpp
class Solution {
public:
    int maximalSquare(vector<vector<char>>& mat) {
        vector<vector<int>> dp(mat.size(), vector<int>(mat[0].size()));
        int ans = 0;
        for (int i = 0; i < mat.size(); i++) {
            for (int j = 0; j < mat[0].size(); j++) {
                if (i == 0 || j == 0) dp[i][j] = mat[i][j] - '0';
                else if (mat[i][j] == '1') dp[i][j] = min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]}) + 1;
                ans = max(ans, dp[i][j] * dp[i][j]);
            }
        }
        return ans;
    }
};
```

#### 146. 123. Best Time to Buy and Sell Stock III

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete at most two transactions.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
 

Example 1:

Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
Example 2:

Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
Example 3:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (!prices.size()) return 0;
        int n = prices.size();
        int left[n], right[n];
        int leftmin = prices[0], rightmax = prices[n - 1], maxprofit = 0;
        left[0] = 0;
        right[n - 1] = 0;
        for (int i = 1, j = n - 2; i < n, j >= 0; i++, j--) {
            leftmin = min(leftmin, prices[i]);
            left[i] = max(left[i - 1], prices[i] - leftmin);
            rightmax = max(rightmax, prices[j]);
            right[j] = max(right[j + 1], rightmax - prices[j]);
        }
        for (int i = 0; i < n; i++) {
            maxprofit = max(maxprofit, left[i] + right[i]);
        }
        return maxprofit;
    }
};
```

#### 147. 188. Best Time to Buy and Sell Stock IV

You are given an integer array prices where prices[i] is the price of a given stock on the ith day, and an integer k.

Find the maximum profit you can achieve. You may complete at most k transactions: i.e. you may buy at most k times and sell at most k times.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
 

Example 1:

Input: k = 2, prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
Example 2:

Input: k = 2, prices = [3,2,6,5,0,3]
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        vector<vector<vector<int>>> dp(
            n + 1, vector<vector<int>>(2, vector<int>(k + 1, 0)));
        for (int ind = n - 1; ind >= 0; ind--) {
            for (int buy = 0; buy <= 1; buy++) {
                for (int cap = 1; cap <= k; cap++) {
                    if (buy == 1) {
                        dp[ind][buy][cap] =
                            max(-prices[ind] + dp[ind + 1][0][cap],
                                0 + dp[ind + 1][1][cap]);
                    } else {
                        dp[ind][buy][cap] =
                            max(prices[ind] + dp[ind + 1][1][cap - 1],
                                0 + dp[ind + 1][0][cap]);
                    }
                }
            }
        }
        return dp[0][1][k];
    }
};
```

#### 148. 433. Minimum Genetic Mutation

A gene string can be represented by an 8-character long string, with choices from 'A', 'C', 'G', and 'T'.

Suppose we need to investigate a mutation from a gene string startGene to a gene string endGene where one mutation is defined as one single character changed in the gene string.

For example, "AACCGGTT" --> "AACCGGTA" is one mutation.
There is also a gene bank bank that records all the valid gene mutations. A gene must be in bank to make it a valid gene string.

Given the two gene strings startGene and endGene and the gene bank bank, return the minimum number of mutations needed to mutate from startGene to endGene. If there is no such a mutation, return -1.

Note that the starting point is assumed to be valid, so it might not be included in the bank.
 

Example 1:

Input: startGene = "AACCGGTT", endGene = "AACCGGTA", bank = ["AACCGGTA"]
Output: 1
Example 2:

Input: startGene = "AACCGGTT", endGene = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
Output: 2

```cpp
class Solution {
public:
    int minMutation(string start, string end, vector<string>& bank) {
        unordered_set<string> st{bank.begin(), bank.end()};
        if (!st.count(end)) return -1;
        queue<string> Q;
        Q.push(start);
        int steps = 0, s;
        string cur, t;
        while (!Q.empty()) {
            s = Q.size();
            while (s--) {
                cur = Q.front();
                Q.pop();
                if (cur == end) return steps;
                st.erase(cur);
                for (int i = 0; i < 8; i++) {
                    t = cur;
                    t[i] = 'A';
                    if (st.count(t)) Q.push(t);
                    t[i] = 'C';
                    if (st.count(t)) Q.push(t);
                    t[i] = 'G';
                    if (st.count(t)) Q.push(t);
                    t[i] = 'T';
                    if (st.count(t)) Q.push(t);
                }
            }
            steps++;
        }
        return -1;
    }
};
```

#### 149. 909. Snakes and Ladders

You are given an n x n integer matrix board where the cells are labeled from 1 to n2 in a Boustrophedon style starting from the bottom left of the board (i.e. board[n - 1][0]) and alternating direction each row.

You start on square 1 of the board. In each move, starting from square curr, do the following:

Choose a destination square next with a label in the range [curr + 1, min(curr + 6, n2)].
This choice simulates the result of a standard 6-sided die roll: i.e., there are always at most 6 destinations, regardless of the size of the board.
If next has a snake or ladder, you must move to the destination of that snake or ladder. Otherwise, you move to next.
The game ends when you reach the square n2.
A board square on row r and column c has a snake or ladder if board[r][c] != -1. The destination of that snake or ladder is board[r][c]. Squares 1 and n2 are not the starting points of any snake or ladder.

Note that you only take a snake or ladder at most once per dice roll. If the destination to a snake or ladder is the start of another snake or ladder, you do not follow the subsequent snake or ladder.

For example, suppose the board is [[-1,4],[-1,3]], and on the first move, your destination square is 2. You follow the ladder to square 3, but do not follow the subsequent ladder to 4.
Return the least number of dice rolls required to reach the square n2. If it is not possible to reach the square, return -1.
 

Example 1:

Input: board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
Output: 4
Explanation: 
In the beginning, you start at square 1 (at row 5, column 0).
You decide to move to square 2 and must take the ladder to square 15.
You then decide to move to square 17 and must take the snake to square 13.
You then decide to move to square 14 and must take the ladder to square 35.
You then decide to move to square 36, ending the game.
This is the lowest possible number of moves to reach the last square, so return 4.
Example 2:

Input: board = [[-1,-1],[-1,3]]
Output: 1

```cpp
class Solution {
public:
    int snakesAndLadders(vector<vector<int>>& board) {
        int n = board.size(), lbl = 1;
        vector<pair<int, int>> cells(n * n + 1);
        vector<int> columns(n);
        iota(columns.begin(), columns.end(), 0);
        for (int row = n - 1; row >= 0; row--) {
            for (int column : columns) {
                cells[lbl++] = {row, column};
            }
            reverse(columns.begin(), columns.end());
        }
        vector<int> dist(n * n + 1, -1);
        dist[1] = 0;
        queue<int> q;
        q.push(1);
        while (!q.empty()) {
            int curr = q.front();
            q.pop();
            for (int next = curr + 1; next <= min(curr + 6, n * n); next++) {
                auto [row, column] = cells[next];
                int destination = board[row][column] != -1 ? board[row][column] : next;
                if (dist[destination] == -1) {
                    dist[destination] = dist[curr] + 1;
                    q.push(destination);
                }
            }
        }
        return dist[n * n];
    }
};
```

#### 150. 127. Word Ladder

A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

Every adjacent pair of words differs by a single letter.
Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
sk == endWord
Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.
 

Example 1:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.
Example 2:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence

```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end());
        if (dict.find(endWord) == dict.end()) return 0;
        unordered_set<string> beginSet{beginWord}, endSet{endWord};
        int level = 1;
        while (!beginSet.empty() && !endSet.empty()) {
            if (beginSet.size() > endSet.size()) swap(beginSet, endSet);
            unordered_set<string> nextSet;
            for (auto word : beginSet) {
                for (int i = 0; i < word.size(); i++) {
                    char original = word[i];
                    for (char c = 'a'; c <= 'z'; c++) {
                        word[i] = c;
                        if (endSet.find(word) != endSet.end()) return level + 1;
                        if (dict.find(word) != dict.end()) {
                            nextSet.insert(word);
                            dict.erase(word);
                        }
                    }
                    word[i] = original;
                }
            }
            beginSet = nextSet;
            level++;
        }
        return 0;
    }
};
```


### Complete (-_-)
