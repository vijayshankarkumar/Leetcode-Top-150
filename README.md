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
