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
### 2.1 算法描述
归并排序采用的是分治法（Divide and Conquer）思想。基本步骤是：
1. 拆分：将原序列划分成子序列，然后假设子序列已经有序；
2. 合并：将有序的子序列合并，得到完全有序的序列；

归并排序具体有两种思路：一种是自上而下递归地归并，另一种是自下而上迭代地归并。

时间复杂度：最坏、平均时间复杂度均为O(nlogn)。

### 2.2 代码
这里给出归并排序的典型应用：链表排序的代码，采用的是自上而下归并。
``` C++
ListNode* mergeSort(ListNode *head){
    if(!head || !head -> next) return head;

    ListNode *pre_slow = NULL, *slow = head, *fast = head;
    while(fast && fast -> next){
        pre_slow = slow;
        slow = slow -> next;
        fast = fast -> next -> next;
    }
    pre_slow -> next = NULL; // 1.1 divide into sublists
    ListNode *l1 = mergeSort(head); 1.2 mergeSort sublists
    ListNode *l2 = mergeSort(slow);

    return merge2SortedLists(l1, l2); // 2. merge
}

ListNode* merge2SortedLists(ListNode *l1, ListNode *l2){
    ListNode *head = new ListNode(0), *p = head;

    while(l1 && l2){
        if(l1 -> val <= l2 -> val){
            p -> next = l1;
            p = p -> next;
            l1 = l1 -> next;
        }
        else{
            p -> next = l2;
            p = p -> next;
            l2 = l2 -> next;
        }
    }

    if(l1) p -> next = l1;
    if(l2) p -> next = l2;
    p = head -> next; delete head;
    return p;
}
```
