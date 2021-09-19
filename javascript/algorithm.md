# js十大排序算法

## 冒泡排序

步骤：

> 1.依次比较相邻的两个数，如果不符合排序规则，则调换两个数的位置。这样一遍比较下来，能够最大保证最大的数排在最后
> 
> 2.在对最后一位以外的数组，重复前面的过程，直至全部排序完成
> 

javascript 代码

```
funciton bubbleSort(arr) {
    var len= arr.length
    //先循环
    for(var i =0; i < len -1; i++) {
        for(var j = 0; j< len -1 - j; j++){
            if(arr[j] > arr[j+1]) {
                //swap
                var temp = arr[j]
                arr[j] = arr[j+1];
                arr[j + 1] = temp
            }
        }
    }
    
    return arr
}

```

## 2.选择排序

选择排序是一种简单直观的排序算法，时间复杂度为O(n2)

步骤：

> 首先在未排序序列中找到最小（大）元素，存放到排序序列中起始位置
> 
> 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾
> 
> 重复第二步，直到所有元素排序完毕

js代码

```
function selectionSort(arr) {
    var len = arr.lenght,
        minIndex;
    
    for (var i = 0 ; i < len; i++){
        //假设当前位置i为最小（最大）
        min = i;
        for (var j = j+1; j<len; j++){
            if(arr[j] < arr[minIndex]){
                minIndex = j
            }
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    
    return arr
}

```

## 3.插入排序

插入排序是一种简单直观的排序算法，它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置插入

步骤：


> 1.将第一待排序序列第一个元素看做一个有序序列，把第二个到剩下的元素当作未排序序列
> 
> 2.从头到尾依次扫描未排序序列，将扫描的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面）

js代码实现

```
function insertionSort(arr){
    var len = arr.length,
        preIndex,
        current;
    preIndex = i -1;
    current = arr[i];
    while( preIndex >=0 && arr[preIndex] > current){
        arr[preIndex + 1] = arr[preIndex]
        preIndex--
    }
    arr[preIndex+1] = current;
    
    return arr;
}

```

## 4.希尔排序

希尔排序，也称递减增量排序算法，是插入排序的一种高效的改进版本。但是希尔排序是非稳定的排序算法

希尔排序是基于插入排序的以下两点性质而提出改进方法：

1.插入排序在对几乎应排好序的数据操作时，效率高，即可以达到线性排序的效率

2.但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位

希尔排序的基本思想是： 先将整个待排序的记录序列分割为若干子序列分别进行直接插入排序，待整个序列中的记录"基本有序"时，再对全体记录进行依次直接插入排序

算法步骤

> 1.选择一个增量序列t1, t2, ...., tk, 其中ti > tj, tk = 1;
> 
> 2.按增量序列个数k, 对序列进行 k 趟排序;
> 
> 3.每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。


javascript代码实现

```
function shellSort (arr) {
    var len = arr.length,
        temp,
        gap = 1;
    while(gap < len/3) {  //动态定义间隔序列
        gap = gap*3 +1;
    }
    for (gap; gap > 0 ; gap = Math.floor(gap/3)) {
        for (var i = gap; i < len; i++ ) {
            temp = arr[i];
            for (var j = i - gap; j >=0 && arr[j] > temp; j-=gap ) {
                arr[j+gap] = arr[j]
            }
            arr[j+gap] = temp;
        }
    }
    return arr;
}

```

## 5.归并排序




