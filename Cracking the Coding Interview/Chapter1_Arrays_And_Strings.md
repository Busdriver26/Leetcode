# 01.01	判定字符是否唯一  

20210217

> 实现一个算法，确定一个字符串 s 的所有字符是否全都不同。
>
> 示例 1：
>
> 输入: s = "leetcode"
> 输出: false 
> 示例 2：
>
> 输入: s = "abc"
> 输出: true
> 限制：
>
> 0 <= len(s) <= 100
> 如果你不使用额外的数据结构，会很加分。

题解：

```c++
class Solution {
public:
    bool isUnique(string astr) {
        int alpha[200] = {0};
        for(int i = 0; i<astr.length();i++){
            alpha[astr[i]] += 1;
            if(alpha[astr[i]]>1){
                return false;
            }
        }
        return true;
    }
};
```

其实可以用位运算（但是没规定26个字母）



# 01.02. 判定是否互为字符重排

20210217

> 给定两个字符串 s1 和 s2，请编写一个程序，确定其中一个字符串的字符重新排列后，能否变成另一个字符串。
>
> 示例 1：
>
> 输入: s1 = "abc", s2 = "bca"
> 输出: true 
> 示例 2：
>
> 输入: s1 = "abc", s2 = "bad"
> 输出: false
> 说明：
>
> 0 <= len(s1) <= 100
> 0 <= len(s2) <= 100

题解：

```c++
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        sort(s1.begin(),s1.end());
        sort(s2.begin(),s2.end());
        return s1==s2;
    }
};
```

想法：

O(nlogn)并且不需要额外开销的做法

# 01.03 String to URL LCCI

20210216

> URL化。编写一种方法，将字符串中的空格全部替换为%20。假定该字符串尾部有足够的空间存放新增字符，并且知道字符串的“真实”长度。（注：用Java实现的话，请使用字符数组实现，以便直接在数组上操作。）
>
> 
>
> 示例 1：
>
> 输入："Mr John Smith    ", 13
> 输出："Mr%20John%20Smith"
> 示例 2：
>
> 输入："               ", 5
> 输出："%20%20%20%20%20"
>
>
> 提示：
>
> 字符串长度在 [0, 500000] 范围内。

题解：

```c++
class Solution {
public:
    string replaceSpaces(string S, int length) {
        int cnt = 0;
        for(int i=0;i<length;i++){
            if(S[i]==' '){
                cnt++;
            }
        }
        int new_len = length + 2*cnt;
        int i,j;
        for(i = new_len-1,j=length-1;i>=0;i--,j--){
            if(S[j]==' '){
                S[i]='0';
                i--;
                S[i]='2';
                i--;
                S[i]='%';
            }
            else{
                S[i]=S[j];
            }
        }
        return S.substr(0,new_len);
    }
};
```

想法：

从后往前遍历，滑动窗口即可。

# 01.04. Palindrome Permutation LCCI

Given a string, write a function to check if it is a permutation of a palindrome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.

Example1:

Input: "tactcoa"
Output: true（permutations: "tacocat"、"atcocta", etc.）

```cpp
class Solution {
public:
    bool canPermutePalindrome(string s) {
        int centre = 0;
        sort(s.begin(),s.end());
        for(int i=0;i<s.length();i++){
            if(s[i]==s[i+1]){
                i++;
            }
            else{
                if(centre == 1){
                    return false;
                }
                centre = 1;
            }
        }
        return true;
    }
};
```

# 01.05. One Away LCCI

There are three types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.

Example 1:

Input: 
first = "pale"
second = "ple"
Output: True

Example 2:

Input: 
first = "pales"
second = "pal"
Output: False

```cpp
class Solution {
public:
    bool oneEditAway(string first, string second) {
        string lng=first,sht=second;
        if(second.length()>first.length()){
            lng = second;
            sht = first;
        }
        if(lng.size()-sht.size()>1){
            return false;
        }
            //check replace
        int flag = 0;
        int offset = 0;
        for(int i=0;i<sht.size();i++){
            if(lng[i+offset]!=sht[i]){
                if(flag == 1){
                    return false;
                }
                flag = 1;
                if(sht.size()!=lng.size()){
                    offset = 1;
                    i--;
                }
            }
        }
        return true;
    }
};
```

# 01.06. Compress String LCCI

Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2blc5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).

Example 1:

Input: "aabcccccaaa"
Output: "a2b1c5a3"
Example 2:

Input: "abbccd"
Output: "abbccd"
Explanation: 
The compressed string is "a1b2c2d1", which is longer than the original string.


Note:

0 <= S.length <= 50000

```c++
class Solution {
public:
    string compressString(string S) {
        string newS="";
        int sz = S.size();
        if(sz == 0){
            return newS;
        }
        char last = S[0];
        int cnt = 1;
        for(int i=1;i<sz;i++){
            if(S[i]!=last){
                newS += last+to_string(cnt);
                cnt = 1;
                last = S[i];
            }
            else{
                cnt++;
            }
        }
        newS += last+to_string(cnt);
        if(newS.size()>=sz){
            return S;
        }
        return newS;
    }
};
```

# 01.07. Rotate Matrix LCCI

Given an image represented by an N x N matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

 

Example 1:

Given matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

Rotate the matrix in place. It becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
Example 2:

Given matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

Rotate the matrix in place. It becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        //转置加对称
        //a[i][j]->a[j][i]->a[j][length-i]
        int temp = 0;
        int sz = matrix.size();
        for(int i=0;i<sz;i++){
            for(int j=i+1;j<sz;j++){
                temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        for(int i=0;i<sz;i++){
            for(int j=0;j<sz/2;j++){
                temp = matrix[i][j];
                matrix[i][j] = matrix[i][sz-j-1];
                matrix[i][sz-j-1] = temp;
            }
        }
    }
};
```

# 01.08. Zero Matrix LCCI

Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column are set to 0.

 

Example 1:

Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
Example 2:

Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        const int row = matrix.size();
        const int col = matrix[0].size();
        int zc = 0, zr = 0;
        for(int i=0;i<col;i++){
            if(matrix[0][i]==0){
                zc = 1;
                break;
            }
        }
        for(int i=0;i<row;i++){
            if(matrix[i][0]==0){
                zr = 1;
                break;
            }
        }
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(matrix[i][j]==0){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for(int i=1;i<row;i++){
            for(int j=1;j<col;j++){
                if(matrix[i][0]==0 || matrix[0][j]==0){
                    matrix[i][j]=0;
                }
            }
        }
        if(zc == 1){
            for(int i=0;i<col;i++){
                matrix[0][i] = 0;
            }
        }
        if(zr == 1){
            for(int i=0;i<row;i++){
                matrix[i][0] = 0;
            }
        }
    }
};
```

# 01.09. String Rotation LCCI

Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 (e.g.,"waterbottle" is a rotation of"erbottlewat"). Can you use only one call to the method that checks if one word is a substring of another?

Example 1:

Input: s1 = "waterbottle", s2 = "erbottlewat"
Output: True
Example 2:

Input: s1 = "aa", s2 = "aba"
Output: False

```c++
class Solution {
public:
    bool isFlipedString(string s1, string s2) {
        if(s1.size()!=s2.size())return false;
        if(s1.size()==0)return true;
        string s3 = s2+s2;
        return s3.find(s1)!=string::npos;
    }
};
```

