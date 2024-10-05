# Good dsa problems

<!--❓ ...
- ... <br>
- Optimization : ...
<details>
<summary>Optimal code</summary>
  
```cpp []
  code
```
</details>
-->

❓ Divide Players Into Teams of Equal Skill
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



❓ Permutation in String
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


