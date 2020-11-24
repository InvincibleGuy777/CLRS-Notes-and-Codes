# Problem

Explain how to implement two stacks in one array A\[1..n\] in such a way that
neither stack overflows unless the total number of elements in both stacks together
is n. The PUSH and POP operations should run in O(1) time.

# Code

### `cpp`

```cpp
/*
    content: CLRS Algo in Chapter 10 Ex10.1-2 --> implement of ShareStack
    date-created: 2020-11-24
    date-updated: 2020-11-24
    Editor: VS Code 
    Compiler: g++
    Debugger: gdb
    Version : C++11
    author: InvincibleGuy777  (Chenchen Xu)
    github url:  https://github.com/InvincibleGuy777/CLRS-Notes-and-Codes
    e-mail: 2540588513@qq.com  / a2540588513@stu.xjtu.edu.cn
    csdn blog url: https://me.csdn.net/weixin_42430021
*/

#include <iostream>
using namespace std;

typedef int elem;

const int MaxSize = 6;

// 共享栈
typedef struct{
    elem stack[MaxSize];
    int top1, top2; 
} ShareStack;

void useShareStack();
// operations
bool InitShareStack(ShareStack &sstack);
bool ShareStackEmpty(const ShareStack& sstack, int stack_idx);
bool ShareStackFull(const ShareStack& sstack);
bool Push(ShareStack& sstack, int stack_idx, elem x); 
bool Pop(ShareStack &sstack, int stack_idx, elem &out);
void ShowShareStack(const ShareStack &sstack);


int main(){
    useShareStack();
    cout << "Done!" << endl;
    cin.get();
    cin.get();
    return 0;
}

void useShareStack(){
    ShareStack sstack;
    InitShareStack(sstack);
    elem x;
    int stack_idx = 1;
    int choice;  // 1 - push 2 - pop
    elem out;
    bool flag;
    cout << "ShareStack MaxSize = " <<MaxSize<< endl;
    /* Here we assume all inputs are valid. */
    cout << "Start:" << endl;
    cout << "Push (press 1) or Pop (press 2), other to quit: ";
    cin >> choice;
    while (choice > 0 && choice < 3){
        if(choice == 1){ // push
            cout << "Select your stack to push (1 or 2): #";
            cin >> stack_idx;
            cout << "Push x: ";
            cin >> x;
            flag = Push(sstack, stack_idx, x);
            if(flag){
                cout << "============After Push============" << endl;
                ShowShareStack(sstack);
            }
            else
                cout << "Stack is full now" << endl;
        }
        else{ // pop
            cout << "Select your stack to pop (1 or 2): #";
            cin >> stack_idx;
            flag = Pop(sstack, stack_idx, out);
            if(flag){
                cout << "============After Pop=============" << endl;
                ShowShareStack(sstack);
            }
            else
                cout << "Stack #" << stack_idx << " is empty."<<endl;
        }
        cout << "Push (press 1) or Pop (press 2), other to quit: ";
        cin >> choice;
    }
}

bool InitShareStack(ShareStack &sstack){
    sstack.top1 = -1;
    sstack.top2 = MaxSize;
    return true;
}

bool ShareStackEmpty(const ShareStack& sstack, int stack_idx){
    if(stack_idx == 1)
        return sstack.top1 == -1;
    else
        return sstack.top2 == -1;
}
bool ShareStackFull(const ShareStack& sstack){
    return sstack.top1 + 1 == sstack.top2;
}

bool Push(ShareStack& sstack, int stack_idx, elem x){
    if(ShareStackFull(sstack)) // overflow
        return false;
    else
        if(stack_idx == 1)
            sstack.stack[++sstack.top1] = x;
        else
            sstack.stack[--sstack.top2] = x;
    return true;
}
bool Pop(ShareStack &sstack, int stack_idx, elem &out){
    if(ShareStackEmpty(sstack, stack_idx))
        return false;
    else
        if(stack_idx == 1)
            out = sstack.stack[sstack.top1--];
        else
            out = sstack.stack[sstack.top2++];
    return true;
}

void ShowShareStack(const ShareStack &sstack){
    const elem* arr = sstack.stack;
    cout << "Stack #1: " << endl;
    for (int i = 0; i <= sstack.top1; ++i)
        cout << arr[i] << " ";
    cout << endl;
    cout << "Stack #2: " << endl;
    for (int i = MaxSize-1; i >= sstack.top2; --i)
        cout << arr[i] << " ";
    cout << endl;
}

```



# OUTPUT

```
ShareStack MaxSize = 6
Start:
Push (press 1) or Pop (press 2), other to quit: 2
Select your stack to pop (1 or 2): #1
Stack #1 is empty.
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #2
Push x: 8
============After Push============
Stack #1:

Stack #2:
8
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #1
Push x: 132
============After Push============
Stack #1:
132
Stack #2:
8
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #2
Push x: 2323
============After Push============
Stack #1:
132
Stack #2:
8 2323
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #1
Push x: 18
============After Push============
Stack #1:
132 18
Stack #2:
8 2323
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #1
Push x: 234
============After Push============
Stack #1:
132 18 234
Stack #2:
8 2323
Push (press 1) or Pop (press 2), other to quit: 2
Select your stack to pop (1 or 2): #1
============After Pop=============
Stack #1:
132 18
Stack #2:
8 2323
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #2
Push x: 333
============After Push============
Stack #1:
132 18
Stack #2:
8 2323 333
Push (press 1) or Pop (press 2), other to quit: 2
Select your stack to pop (1 or 2): #1
============After Pop=============
Stack #1:
132
Stack #2:
8 2323 333
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #2
Push x: 3141
============After Push============
Stack #1:
132
Stack #2:
8 2323 333 3141
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #2
Push x: 486
============After Push============
Stack #1:
132
Stack #2:
8 2323 333 3141 486
Push (press 1) or Pop (press 2), other to quit: 1
Select your stack to push (1 or 2): #1
Push x: 2323
Stack is full now
Push (press 1) or Pop (press 2), other to quit: -1
Done!

```