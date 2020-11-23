# Problem 

Given an array of n elements with arbitrary permutation, find the k<sup>th</sup> smallest (k > 0) element in the array **within** O(n).

# Code

### `cpp`

```cpp
/*
    content: CLRS Algo in Chapter 9 --> Randomized-Select 
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
#include <cstdlib> // for swap(), srand() and rand()
#include <ctime> // for time()
using namespace std;

typedef int elem;
int RandomizedPartition(vector<elem>& v, int p, int r);
elem RandomizedSelect(vector<elem>& v, int p, int r, int k);

// just for testing RandomizedPartition, you can ignore it
bool RandomizedQuickSort(vector<elem> &v, int p, int r);


int main() {
    vector<elem> list  = {34, 6, 23, 555, 28462, 23, 6, 6, 367538, 38482, 244, 273, 174};
                // = {};
                // = {2};
                 // = {1,1};
    if(list.size() == 0){
        cout << "list cannot be empty"<<endl;
        return 0;
    }
    cout<<"Before:"<<endl;
    for (auto& item: list)
        cout<<item<<" ";
    cout<<endl;
    
    int k;
    cout << "To get the kth smallest number in the list above, enter k: ";
    cin >> k;
    while (!cin || k<0 || k>list.size()){
        cin.clear();
        while (cin.get() != '\n')
            continue;
        cout << "Invalid input. k should be a integer ranging from 1 to n." << endl;
        cout << "Re: To get the kth smallest number in the list above, enter k: ";
        cin >> k;
    }
    //cout <<"k = "<< k << endl;
    int ans = RandomizedSelect(list, 0, list.size() - 1, k);
    
    
    RandomizedQuickSort(list, 0, list.size()-1);
    cout<<"After permutation by quicksort:"<<endl;
    for (auto& item: list)
        cout<<item<<" ";
    cout<<endl;
    
    cout <<"\n"<< "The " << k << "th smallest number in the array is " << ans<<endl;
    cout<<endl<<"DONE!"<<endl;
    cin.get();
    cin.get();
    return 0;
}

/**=====CORE======**/
elem RandomizedSelect(vector<elem>& v, int p, int r, int k){  // insure p<=r and 1 <= k <= r-p+1
    if (p<r){
        int q = RandomizedPartition(v, p, r);
        int rem = q - p + 1;
        if(rem == k)
            return v[q];
        else if(rem < k)
            return RandomizedSelect(v, q + 1, r, k - rem);
        else
            return RandomizedSelect(v, p, q-1, k);
    }
    else
        return v[p];
}

int RandomizedPartition(vector<elem>& v, int p, int r){ // p<r
    // random select pivot
    srand(time(0));
    int q = rand() % (r - p + 1) + p;
    swap(v[q], v[p]);

    // partition
    elem x = v[p];
    int i = p;
    for (int j = p+1; j <= r; ++j)
        if(v[j]<=x){
            i++;
            swap(v[i], v[j]);
        }
    swap(v[p], v[i]);
    return i;
}

// just for testing RandomizedPartition, you can ignore it
bool RandomizedQuickSort(vector<elem> &v, int p, int r){
    if(p<r){
        int q = RandomizedPartition(v, p, r);
        RandomizedQuickSort(v, p, q - 1);
        RandomizedQuickSort(v, q + 1, r);
        return true;
    }
    return false;
}

```

# OUTPUT
```
Before:
34 6 23 555 28462 23 6 6 367538 38482 244 273 174
To get the kth smallest number in the list above, enter k: 8
After permutation by quicksort:
6 6 6 23 23 34 174 244 273 555 28462 38482 367538

The 8th smallest number in the array is 244

DONE!

```


# Algo Analysis

## COMPLEXITY

**Time** O(N)

**Space** O(1)

See the analysis of running time in detail in the book at 9-2. 

