# Data Structure Experiment Report a

## Experiment 1--quick sort

> 该实验报告将从一下几个方面进行总结
>
> > 算法设计思路
> >
> > 算法代码实现
> >
> > 程序发生错误
> >
> > 程序错误解决

### 算法设计思路

​	在学习了冒泡排序，插入排序等基础排序算法后，我们逐渐意识到其算法对于时间复杂度处理的劣势**因为其每排好一个元素的顺序就需要进行重复不断的移动其他元素的顺序**，因此结合分治的思想，我们尝试运用分而治之的思想来分块进行排序，最终达到整体的有序性。

​	基于**分而治之**的思想，我们的主题思路是：像切蛋糕一样，不断的将序列进行分割，每一段序列再分别进行相同的过程，最终每一个小序列都达到有序，最终使整体有序。那么，我们就遇到几个问题：怎么分？怎么排？

​	通过上面对算法的基本描述，我们不难发现，要想实现这样一种重复循环的过程，我们必将要运用到**递归**的思想。那么定下主题基调，我们来解决第一个问题：怎么分？不能随便拿出一个元素，然后左右分开吧？这样是不可能确保有序的。那么，如何分才能保证有序呢？理想的状态是找到一个不大不小的中间值，他的左边都比他小，右边都比他大。没错，这就是最理想的状态。那么如何达到这种理想状态呢？那就只有不断调整位置了，具体怎么操作我们会在算法代码实现的部分有所讲解。

### 算法代码实现

​	那么，如何将我们的想法代码实现呢？首先，我们不妨先不考虑麻烦的事情，我们假设已经有了一个*partition*的函数，他可以帮我们找到那个中间的pivot(枢纽)；

```c
void qsort(int* a,int low,int high){
    int pivot;
    if(low<high){
        pivot=partition(a,low,high);//这里就是可以帮我们找到关键pivot并排好序列的函数。
        qsort(a,low,pivot-1);//分而治之
        qsort(a,pivot+1,high);
    }
}
```

​	接下来，我们来详细解读一下这个partition函数该如何实现。实现partition函数，整个过程可以说是不断的换位置的过程。找到一个设定的pivot然后取定pivotkey，比他大的就放在右边，比他小的就放在左边。不断的循环，直到左边全是小的，右边全是大的。

```c

int partition(int* a,int low,int high){
    int pivotkey=a[low];//取定枢纽值
    while(low<high){
        while(low<high&&a[high]>=pivotkey){//对于在右边的值，比枢纽大的我们就不必管
            high--;
        }
        int t;t=a[low];a[low]=a[high];a[high]=t;//遇到一个在枢纽左边且比枢纽大的，那我们必须采取措施把他放在右边
        while(low<high&&a[low]<=pivotkey){//之后开始确保左边其他元素顺序的合法性
            low++;
        }
        t=a[low];a[low]=a[high];a[high]=t;//如此往复
    }
    return low;
}
```

### 程序发生错误

* 对于枢纽值的确定出现错误

```mistake
int partition(int* a,int low,int high){
    int pivotkey=a[i];//这里出现错误
这里我在设定枢纽值的时候，本意是想将每段数组的第一个数作为枢纽，但是出现了这个小错误，因为大数组a已经被分为好几个小数组，而pivot应该是每个小数组的第一个值。所以，改正应为
	int pivotkey=a[low];
```

* 对于partition函数循环结束条件出现错误

~~~mistake
while(low<high){
        while(a[high]>=pivotkey){//这里忘记判断基本条件low<high，下同
            high--;
        }
        int t;t=a[low];a[low]=a[high];a[high]=t;
        while(a[low]<=pivotkey){
            low++;
        }
        t=a[low];a[low]=a[high];a[high]=t;
    }
~~~

### 程序错误处理

* 对于枢纽值的处理，分段数组的首元素为a[low];

~~~amended
int partition(int* a,int low,int high){
    int pivotkey=a[i];//这里出现错误
这里我在设定枢纽值的时候，本意是想将每段数组的第一个数作为枢纽，但是出现了这个小错误，因为大数组a已经被分为好几个小数组，而pivot应该是每个小数组的第一个值。所以，改正应为
	int pivotkey=a[low];
~~~

* 对于while循环我们要记住条件的判定，尤其是嵌套循环，尽管外循环进行了限制，但是内循环仍有可能越界。

~~~amended
 while(low<high){
        while(low<high&&a[high]>=pivotkey){//加入基本条件判断low<high
            high--;
        }
        int t;t=a[low];a[low]=a[high];a[high]=t;
        while(low<high&&a[low]<=pivotkey){
            low++;
        }
        t=a[low];a[low]=a[high];a[high]=t;
    }
~~~



























