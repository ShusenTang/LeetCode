# 背包问题

## 1 算法描述

背包问题系列总结见[我的博客](https://tangshusen.me/2019/11/24/knapsack-problem/)。

## 2 代码

``` C++
/*
https://tangshusen.me/2019/11/24/knapsack-problem/
01背包, 完全背包, 多重背包模板(二进制优化). 
2020.01.04 by tangshusen.

用法:
    对每个物品调用对应的函数即可, 例如多重背包:
    for(int i = 0; i < N; i++) 
        multiple_pack_step(dp, w[i], v[i], num[i], W);

参数:
    dp   : 空间优化后的一维dp数组, 即dp[i]表示最大承重为i的书包的结果
    w    : 这个物品的重量
    v    : 这个物品的价值
    n    : 这个物品的个数
    max_w: 书包的最大承重
*/
void zero_one_pack_step(vector<int>&dp, int w, int v, int max_w){
    for(int j = max_w; j >= w; j--) // 反向枚举!!!
        dp[j] = max(dp[j], dp[j - w] + v);
}

void complete_pack_step(vector<int>&dp, int w, int v, int max_w){
    for(int j = w; j <= max_w; j++) // 正向枚举!!!
        dp[j] = max(dp[j], dp[j - w] + v);

    // 法二: 转换成01背包, 二进制优化
    // int n = max_w / w, k = 1;
    // while(n > 0){
    //     zero_one_pack_step(dp, w*k, v*k, max_w);
    //     n -= k;
    //     k = k*2 > n ? n : k*2;
    // }
}

void multiple_pack_step(vector<int>&dp, int w, int v, int n, int max_w){
   if(n >= max_w / w) complete_pack_step(dp, w, v, max_w);
   else{ // 转换成01背包, 二进制优化
       int k = 1;
       while(n > 0){
           zero_one_pack_step(dp, w*k, v*k, max_w);
           n -= k;
           k = k*2 > n ? n : k*2;
       }
   }
}
```