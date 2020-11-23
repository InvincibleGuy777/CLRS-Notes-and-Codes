# Problem 

Given an array of elements with arbitrary permutation, find the maximum and minimum element in the array **within** `3floor(n/2)` times of comparison.

Note that the common algorithm involves `2n-2` times of comparison.


# Code

### `cpp`

```cpp
/*
    content: CLRS Algo in Chapter 9 --> Get the max and min elements with least times of comparison
    date-created: 2020-11-23
    date-updated: 2020-11-23
    Editor: VS Code 
    Compiler: g++
    Debugger: gdb
    Version : C++11
    author: InvincibleGuy777  (Chenchen Xu)
    github url:  https://github.com/InvincibleGuy777/CLRS-Notes-and-Codes
    e-mail: 2540588513@qq.com  / a2540588513@stu.xjtu.edu.cn
    csdn blog url: https://me.csdn.net/weixin_42430021
*/

#include <vector>
#include <iostream>
#include <cmath>
using namespace std;

typedef int elem;
bool getMaxAndMin(const vector<elem>& a, elem& rmax, elem& rmin);

int main() {
    vector<elem> list = {34, 6, 23, 555, 28462, 367538, 38482, 244, 273, 174};
                // = {};
                // = {2};
                // = {5,8};
    cout<<"List:"<<endl;
    for (auto& item: list)
        cout<<item<<" ";
    cout<<endl;
    elem minval, maxval;
    if(!getMaxAndMin(list, maxval, minval))
        cout<<"There's no element in the list!"<<endl;
    else{
       cout<<"Max: "<<maxval<<endl;
       cout<<"Min: "<<minval<<endl; 
    }
    cout<<endl<<"DONE!"<<endl;
    cin.get(); 
    return 0;
}

bool getMaxAndMin(const vector<elem>& a, elem& rmax, elem& rmin){
    int n = a.size();
    if(n==0){
        rmax=rmin=-1;
        return false;
    }
    if(n%2 == 0){ // even
        rmax = max(a[0], a[1]);
        rmin = min(a[0], a[1]);
    }
    else{
        rmin = rmax = a[0];
    }
    for (int i=3-n%2; i<n; i+=2){
        if(a[i] < a[i-1]){
            rmin = rmin<=a[i]?rmin:a[i];
            rmax = rmax>=a[i-1]?rmax:a[i-1];
        }
        else{
            rmin = rmin<=a[i-1]?rmin:a[i-1];
            rmax = rmax>=a[i]?rmax:a[i];
        }
    }
    return true;
}

```
# OUTPUT
```
List:
34 6 23 555 28462 367538 38482 244 273 174
Max: 367538
Min: 6

DONE!

```


# Algo Analysis

## COMPLEXITY

**Time** O(N)

**Space** O(1)

