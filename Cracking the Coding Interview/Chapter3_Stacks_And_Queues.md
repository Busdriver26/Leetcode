# 03.01. Three in One LCCI

Describe how you could use a single array to implement three stacks.

You should implement push(stackNum, value)、pop(stackNum)、isEmpty(stackNum)、peek(stackNum) methods. stackNum is the index of the stack. value is the value that pushed to the stack.

The constructor requires a stackSize parameter, which represents the size of each stack.

Example1:

 Input: 
["TripleInOne", "push", "push", "pop", "pop", "pop", "isEmpty"]
[[1], [0, 1], [0, 2], [0], [0], [0], [0]]
 Output: 
[null, null, null, 1, -1, -1, true]
Explanation: When the stack is empty, `pop, peek` return -1. When the stack is full, `push` does nothing.
Example2:

 Input: 
["TripleInOne", "push", "push", "push", "pop", "pop", "pop", "peek"]
[[2], [0, 1], [0, 2], [0, 3], [0], [0], [0], [0]]
 Output: 
[null, null, null, null, 2, 1, -1, -1]

```c++
class TripleInOne {
public:
    int start[3];
    int *arr;
    int size[3];
    int s[3];
    
    TripleInOne(int stackSize) {
        arr = new int[stackSize*3];
        size[0] = stackSize-1;
        size[1] = stackSize*2-1;
        size[2] = stackSize*3-1;
        s[0]= 0;
        s[1]= stackSize;
        s[2]= stackSize*2;
        start[0] = 0;
        start[1] = stackSize;
        start[2] = stackSize*2;
    }
    
    void push(int stackNum, int value) {
        if(s[stackNum]>size[stackNum]){
            return;
        }
        arr[s[stackNum]]=value;
        s[stackNum]++;
    }
    
    int pop(int stackNum) {
        if(s[stackNum]==start[stackNum]){
            return -1;
        }
        int res;
        res = arr[s[stackNum]-1];
        s[stackNum]--;
        return res;
    }
    
    int peek(int stackNum) {
        if(s[stackNum]==start[stackNum]){
            return -1;
        }
        int res;
        res = arr[s[stackNum]-1];
        return res;
    }
    
    bool isEmpty(int stackNum) {
        if(s[stackNum]==start[stackNum]){
            return true;
        }
        return false;
    }
};

/**
 * Your TripleInOne object will be instantiated and called as such:
 * TripleInOne* obj = new TripleInOne(stackSize);
 * obj->push(stackNum,value);
 * int param_2 = obj->pop(stackNum);
 * int param_3 = obj->peek(stackNum);
 * bool param_4 = obj->isEmpty(stackNum);
 */
```

# 03.02. Min Stack LCCI

How would you design a stack which, in addition to push and pop, has a function min which returns the minimum element? Push, pop and min should all operate in 0(1) time.

Example:

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> return -3.
minStack.pop();
minStack.top();      --> return 0.
minStack.getMin();   --> return -2.

```c++
class MinStack {
public:
    /** initialize your data structure here. */
    int min[10000] = {0};
    int *stack;
    int head = 0;
    MinStack() {
        stack = new int[10000];
        for(int i=0;i<10000;i++){
            min[i] = INT_MAX;
        }
    }
    
    void push(int x) {
        if(head==0 || x<min[head-1]){
            min[head]=x;
        }
        else if(head>0){
            min[head]=min[head-1];
        }
        stack[head] = x;
        head++;
        return;
    }
    
    void pop() {
        if(head==0){
            return;
        }
        head--;
        return;
    }
    
    int top() {
        return stack[head-1];
    }
    
    int getMin() {
        return min[head-1];
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

# 