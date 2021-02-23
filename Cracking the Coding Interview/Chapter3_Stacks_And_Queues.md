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

I used approach 1(fixed division) to solve the problem, it seems to be easier and faster(maybe). I haven't consider Approach 2 which creates a dynamic sized stack. The approach on the book has 155 lines.

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

The solution creates a new class in order to save the minimum number, but I use another array to store it.(Or a stack)

# 03.03. Stack of Plates LCCI

Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stack exceeds some threshold. Implement a data structure SetOfStacks that mimics this. SetOfStacks should be composed of several stacks and should create a new stack once the previous one exceeds capacity. SetOfStacks.push() and SetOfStacks.pop() should behave identically to a single stack (that is, pop() should return the same values as it would if there were just a single stack). Follow Up: Implement a function popAt(int index) which performs a pop operation on a specific sub-stack.

You should delete the sub-stack when it becomes empty. pop, popAt should return -1 when there's no element to pop.

Example1:

 Input: 
["StackOfPlates", "push", "push", "popAt", "pop", "pop"]
[[1], [1], [2], [1], [], []]
 Output: 
[null, null, null, 2, 1, -1]
 Explanation: 
Example2:

 Input: 
["StackOfPlates", "push", "push", "push", "popAt", "popAt", "popAt"]
[[2], [1], [2], [3], [0], [0], [0]]
 Output: 
[null, null, null, null, 2, 1, 3]

```cpp
class StackOfPlates {
public:
    vector<vector<int> > arr;
    vector<int> ptr;
    int lim;
    int now=-1;
    StackOfPlates(int cap) {
        lim = cap;
    }
    
    void push(int val) {
        if(now==-1 || ptr[now]>=lim){
            vector<int> temp;
            arr.push_back(temp);
            ptr.push_back(0);
            now++;
        }
        arr[now].push_back(val);
        ptr[now]++;
    }
    
    int pop() {
        if(lim==0 || now==-1){
            return -1;
        }
        int res=arr[now][ptr[now]-1];
        ptr[now]--;
        arr[now].pop_back();
        if(ptr[now]==0){
            now--;
            ptr.pop_back();
            arr.pop_back();
        }
        return res;
    }
    
    int popAt(int index) {
        if(lim==0 ||index==now){
            return pop();
        }
        if(index>now || now==-1){
            return -1;
        }
        int res=arr[index][ptr[index]-1];
        ptr[index]--;
        arr[index].pop_back();
        if(ptr[index]==0){
            vector<vector<int> >::iterator iter = arr.begin()+index;
            vector<int>::iterator iter2 = ptr.begin()+index;
            arr.erase(iter);
            ptr.erase(iter2);
            now--;
        }
        return res;
    }
};

/**
 * Your StackOfPlates object will be instantiated and called as such:
 * StackOfPlates* obj = new StackOfPlates(cap);
 * obj->push(val);
 * int param_2 = obj->pop();
 * int param_3 = obj->popAt(index);
 */
```

03.03 cost me a lot of time. At first I tried array but it seems to be too complicated.

# 03.04. Implement Queue using Stacks LCCI

Implement a MyQueue class which implements a queue using two stacks.


Example:

MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);
queue.peek();  // return 1
queue.pop();   // return 1
queue.empty(); // return false


Notes:

You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

```cpp
class MyQueue {
public:
    stack<int> s1,s2;
    /** Initialize your data structure here. */
    MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while(!s1.empty()){
            s2.push(s1.top());
            s1.pop();
        }
        int res = s2.top();
        s2.pop();
        while(!s2.empty()){
            s1.push(s2.top());
            s2.pop();
        }
        return res;
    }
    
    /** Get the front element. */
    int peek() {
        while(!s1.empty()){
            s2.push(s1.top());
            s1.pop();
        }
        int res = s2.top();
        while(!s2.empty()){
            s1.push(s2.top());
            s2.pop();
        }
        return res;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

Easy one. Use the second stack to turn the first one around.

# 03.05. Sort of Stacks LCCI

Write a program to sort a stack such that the smallest items are on the top. You can use an additional temporary stack, but you may not copy the elements into any other data structure (such as an array). The stack supports the following operations: push, pop, peek, and isEmpty. When the stack is empty, peek should return -1.

Example1:

 Input: 
["SortedStack", "push", "push", "peek", "pop", "peek"]
[[], [1], [2], [], [], []]
 Output: 
[null,null,null,1,null,2]
Example2:

 Input:  
["SortedStack", "pop", "pop", "push", "pop", "isEmpty"]
[[], [], [], [1], [], []]
 Output: 
[null,null,null,null,null,true]
Note:

The total number of elements in the stack is within the range [0, 5000].

```cpp
class SortedStack {
public:
    stack<int> s1,s2;
    SortedStack() {

    }
    
    void push(int val) {
        while(!s1.empty() && val>=s1.top()){
            s2.push(s1.top());
            s1.pop();
        }
        s1.push(val);
        while(!s2.empty()){
            s1.push(s2.top());
            s2.pop();
        }
        return;
    }
    
    void pop() {
        if(!s1.empty())
            s1.pop();
    }
    
    int peek() {
        if(!s1.empty())
            return s1.top();
        return -1;
    }
    
    bool isEmpty() {
        return s1.empty();
    }
};

/**
 * Your SortedStack object will be instantiated and called as such:
 * SortedStack* obj = new SortedStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->isEmpty();
 */
```

I used the O(n2) way, the simplest way. The solution gives quicksort and merge sort, seems to be interesting.

# 03.06 Animal Shelter LCCI

An animal shelter, which holds only dogs and cats, operates on a strictly"first in, first out" basis. People must adopt either the"oldest" (based on arrival time) of all animals at the shelter, or they can select whether they would prefer a dog or a cat (and will receive the oldest animal of that type). They cannot select which specific animal they would like. Create the data structures to maintain this system and implement operations such as enqueue, dequeueAny, dequeueDog, and dequeueCat. You may use the built-in Linked list data structure.

enqueue method has a animal parameter, animal[0] represents the number of the animal, animal[1] represents the type of the animal, 0 for cat and 1 for dog.

dequeue* method returns [animal number, animal type], if there's no animal that can be adopted, return [-1, -1].

Example1:

 Input: 
["AnimalShelf", "enqueue", "enqueue", "dequeueCat", "dequeueDog", "dequeueAny"]
[[], [[0, 0]], [[1, 0]], [], [], []]
 Output: 
[null,null,null,[0,0],[-1,-1],[1,0]]
Example2:

 Input: 
["AnimalShelf", "enqueue", "enqueue", "enqueue", "dequeueDog", "dequeueCat", "dequeueAny"]
[[], [[0, 0]], [[1, 0]], [[2, 1]], [], [], []]
 Output: 
[null,null,null,null,[2,1],[0,0],[1,0]]
Note:

The number of animals in the shelter will not exceed 20000.

```cpp
class AnimalShelf {
public:
    list<vector<int> > dog,cat;
    vector<int> err;
    vector<int> res;
    list<vector<int>>::iterator it;
    AnimalShelf() {
        dog.clear();
        cat.clear();
        res.clear();
        err.clear();
        err.push_back(-1);
        err.push_back(-1);
    }
    
    void enqueue(vector<int> animal) {
        if(animal[1]==0){
            cat.push_back(animal);
        }
        else{
            dog.push_back(animal);
        }
    }
    
    vector<int> dequeueAny() {
        if(dog.size()==0 && cat.size()==0){
            return err;
        }
        if(cat.size()==0){
            return dequeueDog();
        }
        if(dog.size()==0 || cat.front()[0]<dog.front()[0]){
            return dequeueCat();
        }
        else{
            return dequeueDog();
        }
    }
    
    vector<int> dequeueCat() {
        if(cat.size()==0){
            return err;
        }
        res=cat.front();
        it = cat.begin();
        it = cat.erase(it);
        return res;
    }
    
    vector<int> dequeueDog() {
        if(dog.size()==0){
            return err;
        }
        res=dog.front();
        it = dog.begin();
        it = dog.erase(it);
        return res;
    }
};

/**
 * Your AnimalShelf object will be instantiated and called as such:
 * AnimalShelf* obj = new AnimalShelf();
 * obj->enqueue(animal);
 * vector<int> param_2 = obj->dequeueAny();
 * vector<int> param_3 = obj->dequeueDog();
 * vector<int> param_4 = obj->dequeueCat();
 */
```

The stack and Queue series challenged me not the stack but C++. STL is so different!