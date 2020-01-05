# 最小生成树

## 1. Prim算法
### 1.1 算法描述
TODO

### 1.2 代码

``` C++
int Prim(vector<vector<int> >&G, int start){
    /*
    用Prim算法求最小生成树的代价.
    Prim算法:
        又称"加点法", 每次选择横跨最小生成树内外的边中代价最小的边对应的点, 加入到最小生成树中。
    参数:
        G: 连通图的邻接矩阵
        start: 起始结点编号
    
    时间复杂度O(v^2)
    */
    int n = G.size(); // 顶点数
    int res = 0;

    vector<int>processd(n, 0); // 是否已经被处理(即是否已在生成树中)
    processd[start] = 1;

    vector<int>lowCost(n, 0); // 记录每个结点到生成树结点集合的最短边长度
    for(int i = 0; i < n; i++)
        if(i != start) lowCost[i] = G[start][i];
    
    // 不断加入剩下的 n-1 个结点到生成树结点集合中
    for(int i = 1; i < n; i++){ // 循环 n-1 次
        int mincost = 0x7FFFFFFF, v = -1;
        for(int j = 0; j < n; j++){
            if(!processd[j] && mincost >= lowCost[j]){
                mincost = lowCost[j];
                v = j;
            }
        }
        if(v == -1){
            cout << "Error: 输入的图不是连通图!!!" << endl;
            return -1;
        }
        // 将结点v加入到已处理集合中
        processd[v] = 1; res += mincost;
        // 更新每个结点到生成树结点集合的最短边长度
        for(int j = 0; j < n; j++)
            if(!processd[j] && G[v][j] < lowCost[j])
                lowCost[j] = G[v][j];
    }
    return res;
}
```

## 2. Kruskal算法
TODO