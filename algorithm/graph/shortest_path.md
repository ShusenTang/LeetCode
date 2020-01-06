# 最短路径问题

## 1. Dijkstra算法
## 1.1 算法描述
TODO

## 1.2 代码
``` C++
const int INF = 0x7FFFFFFF;
void Dijkstra(vector<vector<int> >&G, int s, vector<int>&dist, vector<int>&prev){
    /*
    Dijkstra算法求单源最短路径. (与Prim算法很相似, 注意对比)
    
    参数:
        G   : 有向图的邻接矩阵, 注意不能有负边, 权值为0x7FFFFFFF代表无边
        s   : 源点编号
        dist: 存放各个结点到源点的最短路径长度
        prev: 存放各个结点到源点的最短路径上的前驱结点

    时间复杂度O(v^2)
    */
    int n = G.size(); // 顶点数
    
    for(int i = 0; i < n; i++){
        dist[i] = G[s][i];
        prev[i] = -1;
    }
    dist[s] = 0; // 源点到自己最短距离为0
    vector<int>processd(n, 0); // 是否已经被处理(即是否已求得最短路径)
    processd[s] = 1;
    
    // 不断加入剩下的 n-1 个结点到已被处理结点集合中
    for(int i = 1; i < n; i++){ // 循环 n-1 次
        int mindist = INF, v = -1;
        for(int j = 0; j < n; j++){
            if(!processd[j] && mindist > dist[j]){
                mindist = dist[j];
                v = j;
            }
        }
        if(v == -1){
            // cout << "Warning: 存在源点不可达的结点, 提前结束!" << endl;
            return;
        }

        // 将结点v加入到已处理集合中
        processd[v] = 1;
        
        // 更新剩余结点到已处理结点集合的最短距离
        for(int j = 0; j < n; j++)
            if(!processd[j] && G[v][j] < INF && dist[j] - dist[v] > G[v][j]){
                dist[j] = dist[v] + G[v][j]; 
                prev[j] = v;
            }
    }
}
```

## 2. Bellman-Ford算法
TODO

## 3. Floyd算法