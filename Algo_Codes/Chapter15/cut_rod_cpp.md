
### `cpp`

```cpp
/*
    content: CLRS Algo in Chapter 15 Section 15.1  --> implement of "CUT-ROD PROBLEM"
    date-created: 2020-12-05
    date-updated: 2020-12-05
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
#include<vector>
#include<cmath>
using namespace std;
class Solution{
public:
    enum Mode{MEMO, BOTTOMUP};
private:
    typedef struct {int profit; vector<int> cuts;} solution; // profit->最大利润 cuts->切割方案
    ///
    // 备忘录方式的辅助函数
    ///
    int MemoizedCutRodAUX(vector<int>& p, int len, vector<int>& memo){
        if(memo[len] >= 0)
            return memo[len];
        if(len == 0)
            return 0;
        int q = -1;
        for (int i = 0; i < len; i++)
            q = max(q, p[i] + MemoizedCutRodAUX(p, len-i-1, memo));
        memo[len] = q;
        return q;
    }
    ///
    // 备忘录方式的辅助函数，返回最大利润以及切割方案(cuts引用)
    /// 
    int ExtendedMemoizedCutRodAUX(vector<int>& p, 
                        int len, vector<int>& profits, vector<int>& cuts){
        if(profits[len] >= 0)
            return profits[len];
        if(len == 0)
            return 0;
        int q = -1;
        cuts[0] = 0;
        for (int i = 1; i <= len; i++){
            int val = p[i-1] + ExtendedMemoizedCutRodAUX(p, len - i, profits, cuts);
            if(q < val){
                q = val;
                cuts[len] = i;
            }
        }
        profits[len] = q;
        return q;
    }
    ///
    // 自底向上方法，返回最大利润以及切割方案
    ///  
    const solution ExtendedBottomUpCutRod(vector<int>& p, int len){
        solution ret;
        ret.cuts = vector<int>(len + 1);
        vector<int> dp(len + 1);
        dp[0] = 0;
        ret.cuts[0] = 0;
        for (int i = 1; i <= len; i++){
            int q = -1;
            for (int j = 1; j <= i; j++){
                if(q < p[j-1] + dp[i-j]){
                    q = p[j - 1] + dp[i - j];
                    ret.cuts[i] = j;
                }
            }
            dp[i] = q;
        }
        ret.profit = dp[len];
        return ret;
    }
    ///
    // 带备忘录的最优切割，返回最大利润和最有切割方案(solution结构)
    ///
    const solution ExtendedMemoizedCutRod(vector<int>& p, int len){
        vector<int> cuts(len + 1);
        vector<int> memo(len + 1, -1);
        int val = ExtendedMemoizedCutRodAUX(p, len, memo, cuts);
        solution ret;
        ret.cuts = cuts;
        ret.profit = val;
        return ret;
    }
public:
    // recursive method with memo
    int MemoizedCutRod(vector<int>& p, int len){
        vector<int> memo(len + 1, -1);
        return MemoizedCutRodAUX(p, len, memo);
    }

    // bottom up method
    int BottomUpCutRod(vector<int>& p, int len){
        vector<int> dp(len + 1);
        dp[0] = 0;
        for (int i = 1; i<=len; i++){
            int q = -1;
            for (int j=1; j<=i; ++j)
                q = max(q, p[j-1] + dp[i-j]);
            dp[i] = q;
        }
        return dp[len];
    }
    
    // bottom up method with cost
    int BottomUpCutRod(vector<int>& p, int len, int cost=0){
        if(cost < 0)
            cost = 0;
        vector<int> dp(len + 1);
        dp[0] = 0;
        for (int i = 1; i<=len; i++){
            int q = p[i-1];
            for (int j = 1; j < i; j++)
                q = max(q, p[j-1] + dp[i-j] - cost);
            dp[i] = q;
        }
        return dp[len];
    }

    ///
    // 打印长度为 len 的钢条的最优切割方案以及其对应的最大利润
    // mode -> Mode::BOTTOMUP (自底向上方法)，MODE::MEMO (带备忘录递归方法)
    ///
    void PrintCutRodSolution(vector<int>& p, int len, Mode mode=Mode::BOTTOMUP){

        auto ret = mode == Mode::BOTTOMUP ? ExtendedBottomUpCutRod(p, len) 
                                    : ExtendedMemoizedCutRod(p, len);
        cout << "cutting method:\n";
        while (len > 0){
            cout << ret.cuts[len] << "->";
            len -= ret.cuts[len];
        }
        cout << "###" << endl;
        cout << "max profit: " << ret.profit << endl;
    }
};

int main(){
    vector<int> prices = {1, 5, 8, 9, 10, 17, 17, 20, 24, 30};
    Solution sol;
    int len = 1;
    cout << "The prices for each length: " << endl;
    cout << "Len: \t";
    for (int i = 0; i < static_cast<int>(prices.size()); i++)
        cout << i + 1 <<'\t';
    cout << "\nPrice: \t";
    for (int i = 0; i < static_cast<int>(prices.size()); i++)
        cout << prices[i] <<'\t';
    cout << "\nEnter the length: ";
    while (len >= 0){
        cin >> len;
        while (!cin){
            cin.clear();
            while (cin.get() != '\n')
                continue;
            cout << "The rod length must be a integer!"<<endl;
            cout << "Enter the length: ";
            cin >> len;
        }
        if(len < 0)
            break;
        // filter
        while (cin.get() != '\n')
            continue;
        int cost;
        cout << "Enter the cost: ";
        cin >> cost;
        while (!cin){
            cin.clear();
            while (cin.get() != '\n')
                continue;
            cout << "The cost must be a integer!"<<endl;
            cout << "Enter the cost: ";
            cin >> cost;
        }
        //cout<<"The maximum profits is "<<sol.MemoizedCutRod(prices, len)<<endl;
        //cout<<"The maximum profits is "<<sol.BottomUpCutRod(prices, len)<<endl;
        //sol.PrintCutRodSolution(prices, len);
        cout << "The maximum profits is " << sol.BottomUpCutRod(prices, len, cost)<< endl;
        // sol.PrintCutRodSolution(prices, len, Solution::MEMO);
        // filter
        while (cin.get() != '\n')
            continue;
        cout << "Enter the length: ";
    }
    cout << "Done." << endl;
    cin.get();
    cin.get();
    return 0;
}
```