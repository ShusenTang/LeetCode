# 最小生成树
最小生成树（minimum spanning tree, MST）是连通加权无向图中一棵权值最小的生成树。无向图的生成树是具有图的全部顶点，但边数最少的连通子图。

练习题: [POJ 1258:Agri-Net](http://bailian.openjudge.cn/practice/1258/)

## 1. Prim算法
### 1.1 算法描述
Prim算法又称"加点法", 从只包含一个顶点的生成树开始, Prim算法每次贪心地选择一个离当前生成树最近的点加入到生成树, 直到加入连通图的所有顶点。

算法步骤:

1. 输入：加权连通图G，其中顶点集合为V，边集合为E；

2. 初始化: Vnew = {x}，其中x为集合V中的任一节点（起始点），Enew = {} 为空；

3. 重复下列操作，直到Vnew = V:
    a.在集合E中选取权值最小的边<u, v>，其中u为集合Vnew中的元素，而v不在Vnew集合当中；
    b.将v加入集合Vnew中，将<u, v>边加入集合Enew中;

4. 输出：Vnew 和 Enew 即所得到的最小生成树。

实现时，我们可以用一个数组`processd`来记录某个节点是否已经位于集合Vnew中。另外，常见题目都是求最小生成树的代价，所以我们只需再用一个数组`lowCost`来记录那些横跨当前生成树内外的最短边长度，每次加入一个点后都要适当更新`lowCost`。

时间复杂度: 
* 邻接矩阵 O(V^2)
* 邻接表 O(ElogV)

### 1.2 代码

``` C++
const int INF = 0x7FFFFFFF;
int Prim(vector<vector<int> >&G, int start){
    /*
    用Prim算法求最小生成树的代价. (与Dijkstra算法很相似, 注意对比)
    Prim算法:
        又称"加点法", 每次选择横跨当前生成树内外的边中代价最小的边对应的点, 加入到最小生成树中。
    参数:
        G: 连通图的邻接矩阵, 权值为0x7FFFFFFF代表无边
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
        int mincost = INF, v = -1;
        for(int j = 0; j < n; j++){
            if(!processd[j] && mincost > lowCost[j]){
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
        
        // 更新剩余结点到生成树结点集合的最短边长度
        for(int j = 0; j < n; j++)
            if(!processd[j] && G[v][j] < lowCost[j])
                lowCost[j] = G[v][j];
    }
    return res;
}
```

## 2. Kruskal算法

### 2.1 算法描述

Kruskal算法把所有的边按照权值先从小到大排列，接着按照顺序选取每条边，如果这条边的两个端点不属于同一集合，那么就将它们合并，直到所有的点都属于同一个集合为止。

算法步骤：
1. 输入：加权连通图G；
2. 新建图Gnew, 包含G的所有顶点但是无边；
3. 从权值最小的边开始，如果这条边连接的两个节点在Gnew中不在同一个连通分量中，则添加这条边到图Gnew中；
4. 重复3，直至Gnew中所有的节点都在同一个连通分量中。

算法涉及到集合的查询与合并，所以可以使用并查集。

时间复杂度: O(ElogV)

### 2.2 代码

``` C++
vector<int>make_set(int n){
    /*并查集建立*/
    vector<int>father(n);
    for(int i = 0; i < n; i++) father[i] = i;
    return father;
}
int find_set(vector<int>&father, int x){
    /*查询, 带路径压缩*/
    if(father[x] != x)
        father[x] = find_set(father, father[x]);
    return father[x];
}
int union_set(vector<int>&father, int x, int y){
    /*合并, 这里并没有用到这个函数*/
    int xf = find_set(father, x);
    int yf = find_set(father, y);
    if(xf != yf) father[xf] = yf;
}


struct Edge{ // 边定义
    int from, to, len; // from和to从0开始
    Edge(int f, int t, int l){from=f; to=t; len=l;}
    bool operator < (const Edge &e) const{ 
        return len < e.len; 
    }
};
int Kruskal(vector<Edge>&edges, int n){
    /*
    用Kruskal算法求最小生成树的代价.
    Kruskal算法:
        从小到大选取每条边，如果该边的两个端点不属于同一集合，那么就将它们合并，
        直到所有的点都属于同一个集合为止。
    参数:
        edges: 存放所有边的数组
        n    : 图节点总数
    */
    int res = 0;
    vector<int>sets = make_set(n);    // 建立并查集
    sort(edges.begin(), edges.end()); // 所有边排序
    // 遍历每一条边
    for (int i = 0; i < edges.size(); ++i) {
        Edge e = edges[i];
        int x = find_set(sets, e.from);
        int y = find_set(sets, e.to);
        if (x != y){ // 属于不同连通子图, 加入生成树
            res += e.len;
            sets[x] = y; // 合并
        }
    }
    return res;
}
```