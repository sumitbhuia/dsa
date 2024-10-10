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
- LC_String_M_41_`Wrong/Brute-force/If-else` <br>
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
- LC_Stack_30_`Wrong/If-else`_11102024 <br>
- Optimization : `Monotonic stack`
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
