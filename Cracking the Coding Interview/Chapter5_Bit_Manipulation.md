# 05.01. Insert Into Bits LCCI

You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to insert M into N such that M starts at bit j and ends at bit i. You can assume that the bits j through i have enough space to fit all of M. That is, if M = 10011, you can assume that there are at least 5 bits between j and i. You would not, for example, have j = 3 and i = 2, because M could not fully fit between bit 3 and bit 2.

Example1:

 Input: N = 10000000000, M = 10011, i = 2, j = 6
 Output: N = 10001001100
Example2:

 Input:  N = 0, M = 11111, i = 0, j = 4
 Output: N = 11111

```cpp
class Solution {
public:
    int insertBits(int N, int M, int i, int j) {
        M = M<<i;
        for(int k=i;k<=j;k++){
            N &= ~(1<<k);
        }
        return N|M;
    }
};
```

# 05.02. Bianry Number to String LCCI

Given a real number between 0 and 1 (e.g., 0.72) that is passed in as a double, print the binary representation. If the number cannot be represented accurately in binary with at most 32 characters, print "ERROR".

Example1:

 Input: 0.625
 Output: "0.101"
Example2:

 Input: 0.1
 Output: "ERROR"
 Note: 0.1 cannot be represented accurately in binary.
Note:

This two charaters "0." should be counted into 32 characters.

```c++
class Solution {
public:
    string printBin(double num) {
        string res ="0.";
        while(res.length()<=32){
            num*=2;
            if(num==1){
                res+='1';
                return res;
            }
            if(num>1){
                res+='1';
                num-=1;
            }
            else{
                res+='0';
            }
        }
        return "ERROR";
    }
};
```

# 05.03. Reverse Bits LCCI