# Problem

Whereas a stack allows insertion and deletion of elements at only one end, and a
queue allows insertion at one end and deletion at the other end, a deque (double -
ended queue) allows insertion and deletion at both ends. Write four O(1)-time
procedures to insert elements into and delete elements from both ends of a deque
implemented by an array.

# Code

### `cpp`

```cpp
/*
    content: CLRS Algo in Chapter 10 Ex10.1-5 --> implement of "double-ended queue"
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

#include<iostream>
using namespace std;

typedef int elem;

// 双端队列
class Deque{

public:
    enum {MaxSize = 6};
private:
    elem arr[MaxSize];
    int left, right;

public:
    Deque();
    bool isEmpty() const;
    bool isFull() const;
    bool leftEnqueue(elem x);
    bool rightEnqueue(elem x);
    bool leftDequeue(elem &out);
    bool rightDequeue(elem &out);

    void ShowDeque() const;
};

void useDeque();

int main(){
    useDeque();
    cout << "Done!" << endl;
    cin.get();
    cin.get();
    return 0;
}

void useDeque(){
    Deque deque;
    int choice; // 1 -> push 2 -> pop
    int side; // 1 -> left 2 -> right
    elem x;
    elem out;
    bool flag;

    cout << "MaxSize of Deque: " << deque.MaxSize << endl;
    /* Assume that the inputs are all valid. */
    cout << "Enqueue (press 1) or Dequeue (press 2), other to quit: ";
    cin >> choice;
    while (choice > 0 && choice < 3){
        if(choice == 1){ // Enqueue
            cout << "Ready to enqueue ...... " << endl;
            cout << "Left (press 1) or Right (press 2), other to quit: ";
            cin >> side;
            if(side != 1 && side != 2)
                break;
            cout << "Enter the element x: ";
            cin >> x;
            if(side == 1)
                flag = deque.leftEnqueue(x);
            else
                flag = deque.rightEnqueue(x);

            if(flag){
                cout << "=========After Enqueue=========" << endl;
                deque.ShowDeque();
                cout << endl;
            }
            else
                cout << "Enqueue failed! deque is now full." << endl;
        }
        else{ // Dequeue
            cout << "Ready to dequeue ...... " << endl;
            cout << "Left (press 1) or Right (press 2), other to quit: ";
            cin >> side;
            if(side != 1 && side != 2)
                break;
            if(side == 1)
                flag = deque.leftDequeue(out);
            else
                flag = deque.rightDequeue(out);
            
            if(flag){
                cout << out << " is out!" << endl;
                cout << "=========After Dequeue=========" << endl;
                deque.ShowDeque();
                cout << endl;
            }
            else
                cout << "Dequeue failed! deque is now empty!"<<endl;
        }
        cout << "Enqueue (press 1) or Dequeue (press 2), other to quit: ";
        cin >> choice;
    }
}

Deque::Deque(){
    left = right = 0;
}

bool Deque::isEmpty() const{
    return left == right;
}

bool Deque::isFull() const{
    return (right + 1) % MaxSize == left;
}

bool Deque::leftEnqueue(elem x){
    if(isFull())
        return false;
    left = (left - 1 + MaxSize) % MaxSize;
    arr[left] = x;
    return true;
}

bool Deque::rightEnqueue(elem x){
    if(isFull())
        return false;
    arr[right] = x;
    right = (right + 1) % MaxSize;
    return true;
}

bool Deque::leftDequeue(elem& out){
    if(isEmpty())
        return false;
    out = arr[left];
    left = (left + 1) % MaxSize;
    return true;
}

bool Deque::rightDequeue(elem& out){
    if(isEmpty())
        return false;
    right = (right - 1 + MaxSize) % MaxSize;
    out = arr[right];
    return true;
}

void Deque::ShowDeque() const{
    if(isEmpty()){
        cout << "Deque is now empty." <<endl;
        return;
    }
    //cout << left << ", " << right << endl;

    for (int i = 0; i < MaxSize; ++i){
        if(left <= i && (i < right || left > right)
        || left > right && i<right)
            cout << arr[i] << "\t";
        else
            cout << "#"<<"\t";
    }
    cout << endl;

    for (int i = 0; i < MaxSize; ++i)
        if(i == left || i == right)
            cout << "^"<<"\t";
        else
            cout << " "<< "\t";
    cout << endl;

    for (int i = 0; i < MaxSize; ++i)
        if(i == left)
            cout << "left"<<"\t";
        else if(i == right)
            cout << "right"<< "\t";
        else
            cout << "    "<< "\t";
    cout << endl;
}

```



# OUTPUT

```
MaxSize of Deque: 6
Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
Left (press 1) or Right (press 2), other to quit: 1
Dequeue failed! deque is now empty!
Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 375
=========After Enqueue=========
#       #       #       #       #       375
^                                       ^
right                                   left

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 460
=========After Enqueue=========
#       #       #       #       460     375
^                               ^
right                           left

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 2
Enter the element x: 388
=========After Enqueue=========
388     #       #       #       460     375
        ^                       ^
        right                   left

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 793
=========After Enqueue=========
388     #       #       793     460     375
        ^               ^
        right           left

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
Left (press 1) or Right (press 2), other to quit: 2
388 is out!
=========After Dequeue=========
#       #       #       793     460     375
^                       ^
right                   left

Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
Left (press 1) or Right (press 2), other to quit: 2
375 is out!
=========After Dequeue=========
#       #       #       793     460     #
                        ^               ^
                        left            right

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 777
=========After Enqueue=========
#       #       777     793     460     #
                ^                       ^
                left                    right

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 666
=========After Enqueue=========
#       666     777     793     460     #
        ^                               ^
        left                            right

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 555
=========After Enqueue=========
555     666     777     793     460     #
^                                       ^
left                                    right

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 323
Enqueue failed! deque is now full.
Enqueue (press 1) or Dequeue (press 2), other to quit: 2
Ready to dequeue ......
Left (press 1) or Right (press 2), other to quit: 2
460 is out!
=========After Dequeue=========
555     666     777     793     #       #
^                               ^
left                            right

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 369
=========After Enqueue=========
555     666     777     793     #       369
                                ^       ^
                                right   left

Enqueue (press 1) or Dequeue (press 2), other to quit: 1
Ready to enqueue ......
Left (press 1) or Right (press 2), other to quit: 1
Enter the element x: 23
Enqueue failed! deque is now full.
Enqueue (press 1) or Dequeue (press 2), other to quit: 3
Done!

```