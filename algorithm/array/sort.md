# 排序

排序是很常用的算法，尽管STL中有现成的排序实现，但是我们还是应该掌握常见的的排序算法，这里我们给出面试经常遇到的快速排序和归并排序的模板，其他排序算法很简单略。

## 1. 快速排序
### 1.1 算法描述
快排应该是最常用的排序算法了，面试时也经常被要求手撕快排，因此务必掌握，尤其是其核心的划分（partition）思想，在很多场合都能用到。

快速排序使用分治法（Divide and conquer）策略来把待排数组分为两个子数组，然后递归地排序两个子数组。

步骤为：

1. 挑选基准值：随机挑出一个元素（一般选第一个元素就可以了），称为“基准”（pivot），
2. 划分（partition）：重新排序数组，使所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（与基准值相等的数可以到任何一边）。
3. 递归排序子数组：递归地将小于基准值元素的子数组和大于基准值元素的子数组排序；递归出口是子数组大小小于1。

选取基准值有数种具体方法，选取方法对排序的时间复杂度有决定性影响。

时间复杂度: 
* 平均（每次都划分得很均匀）: O(nlogn)
* 最坏（排序数组已为正序或逆序）: O(n^2)

### 1.2 代码

``` C++
int partition(vector<int> &arr, int left, int right){
    /*
    标准的快排划分(非常重要)
    每次选取第一个元素作为pivot
    */
    int pivot = arr[left];
    while(left < right){
        while(left < right && pivot <= arr[right]) right--;
        arr[left] = arr[right]; 
        while(left < right && pivot >= arr[left]) left++;
        arr[right] = arr[left];
    }
    arr[left] = pivot;
    return left; 
}

void quick_sort(vector<int> &arr, int left, int right){
    if(left >= right) return;
    int pivot_i = partition(arr, left, right);
    quick_sort(arr, left, pivot_i - 1);
    quick_sort(arr, pivot_i + 1, right);
}
```

## 2. 归并排序
TODO