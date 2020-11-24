# Problem

Show how to implement a queue using two stacks. Analyze the running time of the
queue operations.

# Code

### `cpp`

```cpp
/*
    content: CLRS Algo in Chapter 10 Ex10.1-6 --> implement of "double-stack queue"
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

class Stack{
public:
    enum{MaxSize = 3};
private:
    elem arr[MaxSize];
    int top;
public:
    Stack();
    bool isEmpty() const;
    bool isFull() const;
    bool Push(elem x);
    bool Pop(elem &out);

    void ShowStack() const;
};

class DSQueue{  // double-stack queue
private:
    Stack st1, st2;  // st2 for enqueue, st1 for dequeue
    // we define that the shift of massive elements occurs only in dequeue process
public:
    bool isEmpty() const;
    bool isFull() const;
    bool Enqueue(elem x);
    bool Dequeue(elem &out);

    int size() const { return st1.MaxSize + st2.MaxSize - 1; }
    void ShowDSQueue() const;
};

void useDSQueue();

int main(){
    useDSQueue();
    cout << "Done!" << endl;
    cin.get();
    cin.get();
    return 0;
}

void useDSQueue(){
    DSQueue dsque;
    cout << "MaxSize of double-stack queue: " << dsque.size() << endl;
    elem x;
    elem out;
    bool flag;
    int choice;
    cout << "Enqueue (press 1) or Dequeue (press 2), other to quit: ";
    cin >> choice;
    while (choice > 0 && choice < 3){
        if(choice == 1){ // Enqueue
            cout << "Ready to enqueue ...... " << endl;
            cout << "Enter an element x: ";
            cin >> x;
            flag = dsque.Enqueue(x);
            if(flag){
                cout << "=========After Enqueue=========" << endl;
                dsque.ShowDSQueue();
                cout << endl;
            }
            else
                cout << "Enqueue failed! DSQueue is now full." << endl;
        }
        else{ // Dequeue
            cout << "Ready to dequeue ...... " << endl;
            flag = dsque.Dequeue(out);
            
            if(flag){
                cout << out << " is out!" << endl;
                cout << "=========After Dequeue=========" << endl;
                dsque.ShowDSQueue();
                cout << endl;
            }
            else
                cout << "Dequeue failed! DSQueue is now empty!"<<endl;
        }
        cout << "Enqueue (press 1) or Dequeue (press 2), other to quit: ";
        cin >> choice;
    }
}


/* Double-stack queue */
bool DSQueue::isEmpty() const{
    return st1.isEmpty() && st2.isEmpty();
}

// actually not necessary to define the function
bool DSQueue::isFull() const{
    return st2.isFull();
}

bool DSQueue::Enqueue(elem x){
    return st2.Push(x);
}
bool DSQueue::Dequeue(elem &out){
    if(isEmpty())
        return false;
    if(!st1.isEmpty())
        return st1.Pop(out);
    else{  // massive elements transfer
        while (!st2.isEmpty()){
            st2.Pop(out);
            st1.Push(out);
        }
        return st1.Pop(out);
    }
}

void DSQueue::ShowDSQueue() const{
    cout << "Stack for enqueue:" << endl;
    st2.ShowStack();
    cout << "Stack for dequeue:" << endl;
    st1.ShowStack();
}

/* Stack */
Stack::Stack(){
    top = -1;
}

bool Stack::isEmpty() const{
    return top == -1;
}

bool Stack::isFull() const{
    return top + 1 == MaxSize;
}

bool Stack::Push(elem x){
    if(isFull())
        return false;
    arr[++top] = x;
    return true;
}

bool Stack::Pop(elem &out){
    if(isEmpty())
        return false;
    out = arr[top--];
    return true;
}

void Stack::ShowStack() const{
    if(isEmpty()){
        cout << "empty"<<endl;
        return;
    }
    for (int i = 0; i <= top; ++i)
        cout << arr[i] << " ";
    cout << endl;
}

```

# Operating time analysis


**Enqueue** O(1) 

**Dequeue** O(N) for the worst case, it happens only when stack1 is empty. For any other case, the operation time is O(1). Hence the **amortized** operation time is O(1).


# OUTPUT

```
MaxSize of double-stack queue: 5
Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
Dequeue failed! DSQueue is now empty!
Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 1
=========After Enqueue=========
Stack for enqueue:
1
Stack for dequeue:
empty

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 2
=========After Enqueue=========
Stack for enqueue:
1 2
Stack for dequeue:
empty

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
1 is out!
=========After Dequeue=========
Stack for enqueue:
empty
Stack for dequeue:
2

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 3
=========After Enqueue=========
Stack for enqueue:
3
Stack for dequeue:
2

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 4
=========After Enqueue=========
Stack for enqueue:
3 4
Stack for dequeue:
2

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 5
=========After Enqueue=========
Stack for enqueue:
3 4 5
Stack for dequeue:
2

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 6
Enqueue failed! DSQueue is now full.
Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
2 is out!
=========After Dequeue=========
Stack for enqueue:
3 4 5
Stack for dequeue:
empty

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
3 is out!
=========After Dequeue=========
Stack for enqueue:
empty
Stack for dequeue:
5 4

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 6
=========After Enqueue=========
Stack for enqueue:
6
Stack for dequeue:
5 4

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 7
=========After Enqueue=========
Stack for enqueue:
6 7
Stack for dequeue:
5 4

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 8
=========After Enqueue=========
Stack for enqueue:
6 7 8
Stack for dequeue:
5 4

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Enter an element x: 9
Enqueue failed! DSQueue is now full.
Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
4 is out!
=========After Dequeue=========
Stack for enqueue:
6 7 8
Stack for dequeue:
5

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
5 is out!
=========After Dequeue=========
Stack for enqueue:
6 7 8
Stack for dequeue:
empty

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
6 is out!
=========After Dequeue=========
Stack for enqueue:
empty
Stack for dequeue:
8 7

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
7 is out!
=========After Dequeue=========
Stack for enqueue:
empty
Stack for dequeue:
8

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
8 is out!
=========After Dequeue=========
Stack for enqueue:
empty
Stack for dequeue:
empty

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
Dequeue failed! DSQueue is now empty!
Enqueue (press 1) or Dequeue (press 2), other to quit: 3
Done!

```

