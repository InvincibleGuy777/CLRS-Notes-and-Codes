# Problem

Show how to implement a stack using two queues. Analyze the running time of the
stack operations.

# Code

### `cpp`

```cpp
/*
    content: CLRS Algo in Chapter 10 Ex10.1-7 --> implement of "double-queue stack"
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

class Queue{
public:
    enum{MaxSize = 5};
private:
    elem arr[MaxSize];
    int front, rear;

public:
    Queue();
    bool isEmpty() const;
    bool isFull() const;
    bool Enqueue(elem x);
    bool Dequeue(elem &out);
    int size() const { return (rear - front + MaxSize) % MaxSize; }

    void ShowQueue() const;
};

class DQStack{  // double-queue stack
private:
    Queue que[2];
    int cur_queue_idx; // the working queue

public:
    DQStack();
    bool isEmpty() const;
    bool isFull() const;
    bool Push(elem x);
    bool Pop(elem &out);
    
    int size() const { return que[0].MaxSize; }
    void ShowDQStack() const;
};

void useDQStack();

int main(){
    useDQStack();
    cout << "Done!" << endl;
    cin.get();
    cin.get();
    return 0;
}

void useDQStack(){
    DQStack dqstack;
    cout << "MaxSize of double-queue stack: " << dqstack.size() << endl;
    elem x;
    elem out;
    bool flag;
    int choice;
    cout << "Push (press 1) or Pop (press 2), other to quit: ";
    cin >> choice;
    while (choice > 0 && choice < 3){
        if(choice == 1){ // Push
            cout << "Ready to push ...... " << endl;
            cout << "Enter an element x: ";
            cin >> x;
            flag = dqstack.Push(x);
            if(flag){
                cout << "=========After Push=========" << endl;
                dqstack.ShowDQStack();
                cout << endl;
            }
            else
                cout << "Push failed! DQStack is now full." << endl;
        }
        else{ // Pop
            cout << "Ready to pop ...... " << endl;
            flag = dqstack.Pop(out);
            
            if(flag){
                cout << out << " is out!" << endl;
                cout << "=========After Pop=========" << endl;
                dqstack.ShowDQStack();
                cout << endl;
            }
            else
                cout << "Pop failed! DQStack is now empty!"<<endl;
        }
        cout << "Push (press 1) or Pop (press 2), other to quit: ";
        cin >> choice;
    }
}

DQStack::DQStack(){
    cur_queue_idx = 0;
}

/* Double-queue stack */
bool DQStack::isEmpty() const{
    return que[cur_queue_idx].isEmpty();
}

bool DQStack::isFull() const{
    return que[cur_queue_idx].isFull();
}

bool DQStack::Push(elem x){
    if(isFull())
        return false;
    return que[cur_queue_idx].Enqueue(x);
}

bool DQStack::Pop(elem &out){
    if(isEmpty())
        return false;
    int cur = cur_queue_idx;
    while (que[cur].size() > 1){
        que[cur].Dequeue(out);
        que[1 - cur].Enqueue(out);
    }
    que[cur].Dequeue(out);
    cur_queue_idx = 1 - cur_queue_idx;
}

void DQStack::ShowDQStack() const{
    cout << "Working Queue #"<< cur_queue_idx+1 << endl;
    que[cur_queue_idx].ShowQueue();
    cout << "Vacant Queue #"<< 2-cur_queue_idx << endl;
    que[1-cur_queue_idx].ShowQueue();
}

/* Queue */
Queue::Queue(){
    front = rear = 0;
}

bool Queue::isEmpty() const{
    return front == rear;
}

bool Queue::isFull() const{
    return (rear + 1) % MaxSize == front;
}

bool Queue::Enqueue(elem x){
    if(isFull())
        return false;
    arr[rear] = x;
    rear = (rear + 1) % MaxSize;
    return true;
}

bool Queue::Dequeue(elem &out){
    if(isEmpty())
        return false;
    out = arr[front];
    front = (front + 1) % MaxSize;
    return true;
}

void Queue::ShowQueue() const{
    if(isEmpty()){
        cout << "empty"<<endl;
        return;
    }
    for (int i = 0; i < MaxSize; ++i){
        if(front <= i && (i < rear || front > rear)
        || front > rear && i < rear)
            cout << arr[i] << "\t";
        else
            cout << "#"<<"\t";
    }
    cout << endl;

    for (int i = 0; i < MaxSize; ++i)
        if(i == front || i == rear)
            cout << "^"<<"\t";
        else
            cout << " "<< "\t";
    cout << endl;

    for (int i = 0; i < MaxSize; ++i)
        if(i == front)
            cout << "front"<<"\t";
        else if(i == rear)
            cout << "rear"<< "\t";
        else
            cout << "    "<< "\t";
    cout << endl<<endl;
}

```

# Operating time analysis

We have two queues and mark one of them as **working**. **PUSH** queues an element on the working queue. **POP** should dequeue all but one element of the working queue and queue them on the **vacant**. The roles of the queues are then reversed, and the final element left in the (now) vacant queue is returned.

*The analysis above refers to <a href="https://walkccc.github.io/CLRS/Chap10/10.1/" target="_blank">walkccc CLRS Solutions.</a>*

**Push** O(1) 

**Pop** O(N) 


# OUTPUT

```
MaxSize of double-queue stack: 5
Push (press 1) or Pop (press 2), other to quit: 2
Ready to pop ......
Pop failed! DQStack is now empty!
Push (press 1) or Pop (press 2), other to quit: 1
Ready to push ......
Enter an element x: 1
=========After Push=========
Working Queue #1
1       #       #       #       #
^       ^
front   rear

Vacant Queue #2
empty

Push (press 1) or Pop (press 2), other to quit: 2
Ready to pop ......
1 is out!
=========After Pop=========
Working Queue #2
empty
Vacant Queue #1
empty

Push (press 1) or Pop (press 2), other to quit: 1
Ready to push ......
Enter an element x: 11
=========After Push=========
Working Queue #2
11      #       #       #       #
^       ^
front   rear

Vacant Queue #1
empty

Push (press 1) or Pop (press 2), other to quit: 1
Ready to push ......
Enter an element x: 22
=========After Push=========
Working Queue #2
11      22      #       #       #
^               ^
front           rear

Vacant Queue #1
empty

Push (press 1) or Pop (press 2), other to quit: 1
Ready to push ......
Enter an element x: 33
=========After Push=========
Working Queue #2
11      22      33      #       #
^                       ^
front                   rear

Vacant Queue #1
empty

Push (press 1) or Pop (press 2), other to quit: 2
Ready to pop ......
33 is out!
=========After Pop=========
Working Queue #1
#       11      22      #       #
        ^               ^
        front           rear

Vacant Queue #2
empty

Push (press 1) or Pop (press 2), other to quit: 1
Ready to push ......
Enter an element x: 44
=========After Push=========
Working Queue #1
#       11      22      44      #
        ^                       ^
        front                   rear

Vacant Queue #2
empty

Push (press 1) or Pop (press 2), other to quit: 1
Ready to push ......
Enter an element x: 55
=========After Push=========
Working Queue #1
#       11      22      44      55
^       ^
rear    front

Vacant Queue #2
empty

Push (press 1) or Pop (press 2), other to quit: 1
Ready to push ......
Enter an element x: 66
Push failed! DQStack is now full.
Push (press 1) or Pop (press 2), other to quit: 2
Ready to pop ......
55 is out!
=========After Pop=========
Working Queue #2
44      #       #       11      22
        ^               ^
        rear            front

Vacant Queue #1
empty

Push (press 1) or Pop (press 2), other to quit: 2
Ready to pop ......
44 is out!
=========After Pop=========
Working Queue #1
11      22      #       #       #
^               ^
front           rear

Vacant Queue #2
empty

Push (press 1) or Pop (press 2), other to quit: 2
Ready to pop ......
22 is out!
=========After Pop=========
Working Queue #2
#       11      #       #       #
        ^       ^
        front   rear

Vacant Queue #1
empty

Push (press 1) or Pop (press 2), other to quit: 2
Ready to pop ......
11 is out!
=========After Pop=========
Working Queue #1
empty
Vacant Queue #2
empty

Push (press 1) or Pop (press 2), other to quit: 3
Done!

```

