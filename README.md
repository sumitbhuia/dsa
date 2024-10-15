# Good dsa problems

<!--
## ❓ ...
- ... <br>
- Optimization : ...
<details>
<summary>Optimal code</summary>
  
```cpp []
  code
```
</details>
-->

## ❓ Separate Black and White Balls
- LC_String_M_`Solved`_151024 <br>
- Optimization : No. of zeros ahead of 1 = No. of swaps required to seperate . 
<details>
<summary>Optimal code</summary>
  
```cpp []
   #pragma GCC optimize("O3", "unroll-loops","Ofast")
class Solution {
public:
    long long minimumSteps(string s) {
        ios_base::sync_with_stdio(0); 
        cin.tie(0); 
        cout.tie(0);

        long long steps = 0 ,cnt=0;
        if (s.length()==1) return 0;

        for(int i = s.length()-1 ; i >= 0  ; i--){
            if(s[i]== '0') cnt++ ;
            else steps += cnt;
        }
        return steps;         
    }
};
```
</details>

## ❓ Maximal Score After Applying K Operations
- LC_Heap_M_`Solved`_141024 <br>
- Optimization : `Max-heap`
<details>
<summary>Optimal code</summary>
  
```cpp []
  #pragma GCC optimize("O3", "unroll-loops","Ofast")
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        ios_base::sync_with_stdio(0); 
        cin.tie(0); 
        cout.tie(0);
        priority_queue<long long> heap(nums.begin(), nums.end());
        long long score = 0 ; 

        for(int i = 0; i<k; i++){
            long long val= heap.top();
            score +=val;
            long long nval = ceil((val/3.0));
            heap.pop();
            heap.push(nval);
        }
        return score;

        
    }
};
```
</details>


## ❓ Divide Players Into Teams of Equal Skill
- LC_Array_M_61_32_`Sort&2Pointer&PairSmallestw/Largest`_04102024<br>
- Optimization : `Frequency array` , bitwise division , handle edge-case early 
<details>
<summary>Optimal code</summary>
  
```cpp []
   long long dividePlayers(vector<int>& skill) {

        int max_skill = 1000;  // Max individual skill value
        vector<int> freq(max_skill + 1, 0); // Precise space allocation
        int teams = skill.size(); // Total players
        long long totalsum = 0;

        // Populate freq. array and total sum of skill[]
        for (int s : skill) {
            totalsum += s; 
            freq[s]++;
        }

        teams >>= 1;  // Bitwise division (total player/2)
        if (totalsum % teams != 0) return -1;  // Total skill cannot be divided equally pairwise.

        int target_pair_skill = totalsum / teams;
        int indv_skill_req = target_pair_skill/2;
        long long chemistry = 0;

        for (int i = 0; i <= indv_skill_req; ++i) {

            if (freq[i] == 0) continue;  // Skip if no players with this skill
            int partner_skill = target_pair_skill - i; // If player preset find partner

            // Case : For player and partner diff. skill 
            if (i != partner_skill) {

                // If both skill not present pair cannot be formed
                if (freq[i] != freq[partner_skill]) return -1;

                // player_skill * partner_skill * freq        
                chemistry += 1LL * i * partner_skill * freq[i];

                freq[partner_skill] = 0;  // Remove partner skill
            }

            // Case : If both partner and player have same skill
            else {
                 // freq[289]=2,4 to make pairs , 2 players with 289 skill
                if (freq[i] % 2 != 0) return -1;

                chemistry+= 1LL * i * i * (freq[i] / 2);
            }
            freq[i] = 0; // Remove player skill
        }

        return chemistry;
    }
```
</details><br>



## ❓ Permutation in String
- LC_String_M_9_6_`FreqArray`_5102024 <br>
- Optimization : `Sliding Window`
- Pattern : Apply sliding window when you need to find pattern/sum in a subarray/substring
<details>
<summary>Optimal code</summary>
  
```cpp []
      bool checkInclusion(string s1, string s2) {
        if (s1.length() > s2.length()) return false;

        vector<int>s1Freq(26,0), windowFreq(26,0);

        // Populate freq array for s1 & first window
            for (int i=0 ; i<s1.length() ; i++){
                s1Freq[s1[i]-'a']++;
                windowFreq[s2[i]-'a']++;
            }

            for(int i = s1.length() ; i<s2.length(); i++){
                if(s1Freq == windowFreq) return true;
                // Slide the window
                windowFreq[s2[i]-'a']++; // Add to right
                windowFreq[s2[i-s1.length()]-'a']--;// Remove from left
            }

     
   
        return s1Freq==windowFreq;
        
    }
```
</details>

## ❓ Maximize the Total Height of Unique Towers
- LC_Array_M_22_`Wrong/MergingSortedArray`_06102024 <br>
- Optimization : Merge and store in a new array in a single for loop in O(log(m+n))
<details>
<summary>Optimal code</summary>
  
```cpp []
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> arr;
        int left = 0, right = 0;

        // Merging the arrays
        while (left < nums1.size() && right < nums2.size()) {
            if (nums1[left] <= nums2[right]) {
                arr.push_back(nums1[left++]);
            } else {
                arr.push_back(nums2[right++]);
            }
        }

        // Add remaining elements
        while (left < nums1.size()) arr.push_back(nums1[left++]);
        while (right < nums2.size()) arr.push_back(nums2[right++]);

        // Find the median
        int size = arr.size();
        if (size % 2 == 0) {
            return (arr[size / 2 - 1] + arr[size / 2]) / 2.0;
        } else {
            return arr[size / 2];
        }
    }


```
</details>

## ❓ Sentence Similarity III
- LC_String_M_5_`Wrong`06102024 <br>
- Optimization : `Split String to Words`
<details>
<summary>Optimal code</summary>
  
```cpp []
  class Solution {
public:
    bool areSentencesSimilar(string sentence1, string sentence2) {
        // If sentence2 is longer, swap them for consistency
        if (sentence1.length() < sentence2.length()) {
            swap(sentence1, sentence2);
        }
        
        // Split sentences into words
        vector<string> words1 = split(sentence1);
        vector<string> words2 = split(sentence2);
        
        int n = words1.size();
        int m = words2.size();
        
        // Check matching words from the start , m is the smaller sentence
        int i = 0;
        while (i < m && words1[i] == words2[i]) {
            i++;
        }
        
        // Check matching words from the end
        int j = 0;
        while (j < m && words1[n - 1 - j] == words2[m - 1 - j]) {
            j++;
        }
        
        // matching from the start , counting matched words , matching from the back counting matched words
        // If all the words from the smaller sentence are continous in the bigger sentence the matched count should be >= to smaller sentence length . Meaning smaller is continuos and words are present in bigger one . 
        return i + j >= m;
    }
    
private:
    // Helper function to split a sentence into words
    vector<string> split(const string& s) {
        istringstream iss(s);
        vector<string> words;
        string word;
        while (iss >> word) {
            words.push_back(word);
        }
        return words;
    }
};
```
</details>

## ❓ Minimum String Length After Removing Substrings
- LC_String_E_3_`Wrong/letter-wise-elimination`_07102024 <br>
- Optimization : `Seperate ans string`,`Push Pop in string ` used as a `stack` . 
<details>
<summary>Optimal code</summary>
  
```cpp []
  #pragma GCC optimize("O3", "unroll-loops","Ofast")
class Solution {
public:
    int minLength(string s) {
        ios_base::sync_with_stdio(0); 
        cin.tie(0); 
        cout.tie(0);


        if(s.length() == 1){ return 1;}

        string ans = "";

        for( auto c : s){
            // if ans is not empty & last element is A & current element is B . Remove A .
            if(!ans.empty() && c=='B' && ans.back()=='A'){ ans.pop_back();}
            // Same for C & D , remove C
            else if(!ans.empty() && c=='D' && ans.back()=='C'){ ans.pop_back();}
            // Else add them to string 'ans'. 
            else{ ans.push_back(c);}

        }
        //Return the size of 'ans'.
        return ans.size();

    }

};
```
</details>


## ❓ Minimum Number of Swaps to Make the String Balanced
- LC_String_M_41_`Wrong/Brute-force/If-else`_081024 <br>
- Optimization : Just count swaps , no actual swaps required , if a bracket is out of place assume it to be swapped and treat it like a swapped item w/ with surrounding . 
<details>
<summary>Optimal code</summary>
  
```cpp []
  #pragma GCC optimize("O3", "unroll-loops","Ofast")
class Solution {
public:
    int minSwaps(string s) {
        ios_base::sync_with_stdio(0); 
        cin.tie(0); 
        cout.tie(0);
        int left_bracket = 0 ,swaps = 0;

    for (char c : s) {
        if (c == ']') {
             if(left_bracket==0) {
                    // Right brkt , assumed as left . If swap happens.
                    left_bracket++;
                    // As there are no left brackets , and finding a right bracket is out of order . So swap needed. 
                    swaps++;
            }
            else{
        // Cancelling non-zero left brkt if right brkt found.
                left_bracket--;
            }
        }
        else{
            // Increase left brkt.
            left_bracket++;
        } 
    }
      return swaps;
    }
};

```
</details>

## ❓ Maximum Width Ramp
- LC_Stack_M_30_`Wrong/If-else`_10102024 <br>
- Optimization : `Monotonic stack`: Stack in decreasing order
<details>
<summary>Optimal code</summary>
  
```cpp []
  #pragma GCC optimize("O3", "unroll-loops","Ofast")
class Solution {
public:
    int maxWidthRamp(vector<int>& nums) {
        ios_base::sync_with_stdio(0); 
        cin.tie(0); 
        cout.tie(0);
        stack<int>st;
        int ans = INT_MIN;
        for(int i =0 ; i<nums.size();i++){
            if(st.empty() ||  nums[st.top()]>nums[i]) st.push(i);
        }

        for(int j = nums.size()-1 ; j>=0 ;j-- ){
            while(!st.empty() && nums[st.top()]<=nums[j]){
                ans = max(ans , j - st.top());
                st.pop();
            }
            
            
        }
        return ans;
       
    }
};
```
</details>

## ❓ The Number of the Smallest Unoccupied Chair
- LC_Heap_M_`O(NlogK)`_`Copied`_111024 <br>
- Optimization : `Min-Heap` , maintain 2 heaps for occupied and unoccupied chairs and constantly update both .
<details>
<summary>Optimal code</summary>
  
```cpp []
  class Solution {
public:
    int smallestChair(vector<vector<int>>& times, int targetFriend) {
        int n = times.size();
        
        // Store the friends with their indices (0, 1, ..., n-1)
        vector<pair<int, int>> friends;
        for (int i = 0; i < n; i++) {
            friends.push_back({times[i][0], i});  // {arrival_time, friend_index}
        }
        
        // Sort the friends by arrival time
        sort(friends.begin(), friends.end());

        // Min-heap to store available chairs (smallest chair numbers)
        priority_queue<int, vector<int>, greater<int>> availableChairs;
        for (int i = 0; i < n; i++) {
            availableChairs.push(i);  // Initially, all chairs are available
        }

        // Min-heap to store occupied chairs and the time they will be free: {leaving_time, chair_number}
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> occupiedChairs;

        for (const auto& [arrivalTime, friendIndex] : friends) {
            // Free up chairs that are now available
            while (!occupiedChairs.empty() && occupiedChairs.top().first <= arrivalTime) {
                availableChairs.push(occupiedChairs.top().second);
                occupiedChairs.pop();
            }
            
            // Assign the smallest available chair to the current friend
            int assignedChair = availableChairs.top();
            availableChairs.pop();
            
            // Record when this friend will leave and free up their chair
            occupiedChairs.push({times[friendIndex][1], assignedChair});
            
            // If this is the target friend, return their chair
            if (friendIndex == targetFriend) {
                return assignedChair;
            }
        }

        return -1;  // This line should never be reached
    }
};
```
</details>

## ❓ Divide Intervals Into Minimum Number of Groups
- LC_M_Prefix-Sum/Heap_`Copied`_121024 <br>
- Optimization : `Prefix sum`, in an vector using `start` and `end+1` intevals as indexes , assign values +1 and -1 . Add them and consecutively and maintain a max.
<details>
<summary>Optimal code</summary>
  
```cpp []
 class Solution {
public:
    int minGroups(vector<vector<int>>& intervals) {
        vector<pair<int, int>> events;
        
        // Convert intervals into events: +1 for start, -1 for end+1
        for (const auto& interval : intervals) {
            events.emplace_back(interval[0], 1);        // Start of an interval
            events.emplace_back(interval[1] + 1, -1);   // End of an interval (non-inclusive)
        }

        // Sort events by time
        sort(events.begin(), events.end());

        int maxGroups = 0, currentGroups = 0;

        // Process events in sorted order
        for (const auto& event : events) {
            currentGroups += event.second;  // Update active groups count
            maxGroups = max(maxGroups, currentGroups);  // Track the max number of groups
        }

        return maxGroups;
    }
};
```
</details>

- Optimization : `min-heap` , the number of overlaps = the number of groups required . Find max number of overlapping intervals.
<details>
<summary>Optimal code</summary>

```cpp []
  class Solution {
public:
    int minGroups(vector<vector<int>>& intervals) {
        // Sort intervals by their start times
        sort(intervals.begin(), intervals.end());

        // Min-heap (priority queue) to store the end times of ongoing groups
        priority_queue<int, vector<int>, greater<int>> pq;

        for (const auto& interval : intervals) {
            int start = interval[0];
            int end = interval[1];

            // If the earliest ending interval ends before the current one starts, remove it
            if (!pq.empty() && pq.top() < start) {
                pq.pop();
            }

            // Add the current interval's end time to the priority queue
            pq.push(end);
        }

        // The size of the priority queue will be the maximum number of overlapping intervals
        return pq.size();
    }
};
```
</details>

## ❓ Smallest Range Covering Elements from K Lists
- LC_Heap_H_`Copied`_131024 <br>
- Optimization :
  - Imagine a 2D matrix , a `sliding window` of the entire first columns ,put the window in a `min-heap` , remove the smallest element from the window and move forward in that list by 1.
  - Maintain a differece of max-min in the window update max everytime .
  - Update max for every new value in the window , and top() in always the min in `min-heap` . 
<details>
<summary>Optimal code</summary>
  
```cpp []
  class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        // Min-Heap: stores (value, list index, element index)
        priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>> minHeap;
        int curMax = INT_MIN;

        // Initialize the heap with the first element of each list
        for (int i = 0; i < nums.size(); i++) {
            minHeap.push({nums[i][0], i, 0});
            curMax = max(curMax, nums[i][0]);
        }
        // Track the smallest range
        vector<int> smallRange = {0,INT_MAX};

        while (!minHeap.empty()) {
            // Get the minimum element from the heap
            vector<int> curr = minHeap.top();
            minHeap.pop();
            int curMin = curr[0], listIdx = curr[1], elemIdx = curr[2];

            // Update the smallest range if a better one is found
            if (curMax - curMin < smallRange[1] - smallRange[0]) {
                smallRange[0] = curMin;
                smallRange[1] = curMax;
            }

            // Move to the next element in the same list
            if (elemIdx + 1 < nums[listIdx].size()) {
                int nextVal = nums[listIdx][elemIdx + 1];
                minHeap.push({nextVal, listIdx, elemIdx + 1});
                curMax = max(curMax, nextVal);
            } else {
                // If any list is exhausted, stop
                break;
            }
        }
        return smallRange;
    }
};
```
</details>



