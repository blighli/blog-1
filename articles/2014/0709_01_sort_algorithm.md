#面试时该死的排序算法

2014-07-09

程序员面试时考算法是个很头疼的问题，但还是不得不看一下。排序分为内部排序和外部排序，内部排序指待排序的记录在内存中，外部排序的记录数量很大，以至于内存放不下而放在外存中，排序过程需要访问外存。这里仅介绍内部排序，包括插入排序、交换排序、选择排序、归并排序、基数排序。


##1 插入排序

####1.1直接插入（straight insertion sort）

算法思路：数组｛k1,k2,……，kn｝，排序一开始k1是一个有序序列，让k2插入得到一个表长为2的有序序列，依此类推，最后让kn插入上述表长为n－1的有序序列，得到表长为n的有序序列。

c实现的代码：
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//    从小到大排序
    int a[]={98,97,34,345,33};
    int k=sizeof(a)/sizeof(a[0]);
    int j;
    for (int i=1; i&lt;k; i++) {
        int temp=a[i];
        for (j=i-1; j&gt;=0&amp;&amp;a[j]&gt;temp; j--) {
            a[j+1]=a[j];
        }
        a[j+1]=temp;
    }



</pre>
####1.2折半插入（binary insertion sort）

算法思路：当直接插入进行到某一趟时，对于r［i］来讲，前面i－1个记录已经按关键字有序。此时不用直接插入排序的方法，而改为折半查找，找出r［i］应插入的位置。

c实现的代码：
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//    从小到大排序
void binasort(int r[100],int n){
    for (int i=1; i&lt;n; i++) {
        int temp =r[i];
        int low=0;
        int high=i-1;
        while (low&lt;=high) {
            int middle=(low+high)/2;
            if (temp&lt;r[middle]) {
                 high=middle-1;
             }else{
                 low=middle+1;
            }
         }
        for (int j=i-1; j&gt;=low; j--) {
            r[j+1]=r[j];
        }
        r[low]=temp;
    }
 }
</pre>
####1.3希尔排序（shell sort）

算法思路：“缩小增量”的排序方法，初期选用增量较大间隔比较，然后增量缩小，最后为1，希尔排序对增量序列没有严格规定。设有组关键字｛99，22，33，333，2，3，23，44｝，由小到大排序，这里n＝8，先第一个个增量取d1=4，那么记录分为4组，第一组r［0］，r［4］，第二组r［1］，r［5］，……在各组内部使用插入排序，使得每组内是有序的，接着取d2=2，分为两组，d3=1,最后就编程有序序列。

c语言实现的代码：
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//    从小到大排序
void shellsort(int r[100],int n){
    int k=n/2;
    while (k&gt;0) {
        for (int i=k; i&lt;n; i++) {
             int temp=r[i];
             int j=i-k;
             while ((r[j]&gt;temp)&amp;&amp;(j&gt;=0)) {
                r[j+k]=r[j];
                j=j-k;
            }
            r[j+k]=temp;

        }
        k/=2;

    }
 }
</pre>
##2 交换排序

####2.1冒泡排序（bubble sort）

算法思路：在排序过程，关键字较小的记录经过与其他记录的对比交换，好像水中的气泡一样，移到数据序列的最前面。

c语言实现的代码：
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//    从小到大排序
void bubblesort(int r[100],int n){
    for (int i=0; i&lt;n-1; i++) {
        for (int j=0; j&lt;n-1-i; j++) {
             if (r[j]&gt;r[j+1]) {
                int temp=r[j];
                r[j]=r[j+1];
                r[j+1]=temp;
            }
        }
    }
}
</pre>
####2.2快速排序(quick sort)

算法思路：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序。整个排序过程可以递归实现，也可以非递归实现。

c语言实现递归的快速排序的代码：
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//    从小到大排序
void quickSort(int a[],int numsize){
    int i=0,j=numsize-1;
    int val=a[0];//指定参考值val大小
    if (numsize&gt;1) { //确保数组长度至少为2，否则无需排序
        while (i&lt;j) {//循环结束条件
 //            从后向前搜索比val小的元素，找到后填到a［i］中并跳出循环
             for (; j&gt;i; j--) {
                if (a[j]&lt;val) {
                    a[i]=a[j];
                    break;
                }
            }
//            从前向后搜索比val大的元素，找到后填到a［j］中并跳出循环
            for (; i&lt;j; i++) {
                 if (a[i]&gt;val) {
                    a[j]=a[i];
                    break;
                }
            }
        }

        a[i]=val;//将保存再val中的数放到a［i］中
        quickSort(a, i);//递归，对前i个数排序
        quickSort(a+i+1, numsize-1-i);//对i＋1到numsize－1-i个数排序
    }

}

</pre>
##3 选择排序

####3.1 简单选择排序（simple selection sort）

算法思路：对于一组关键字｛k1，k2，……kn｝，将其从小到大排序，首先从k1，k2，……k3中选择最小值Kz，在将Kz与k1对换；然后从k2……Kn中选最小值Kz，与k2交换。如此选择和调换n－2趟，第n－1趟只要调换Kn-1 和Kn比较就好了。

c语言实现的代码：
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//从小到大排序
void sisort(int r[100],int n){
    for (int i=0; i&lt;n-1; i++) {
        int z=i;
        for (int j=i+1; j&lt;n; j++) {
             if (r[z]&gt;r[j]) {
                z=j;
            }
        }
        if (z!=i) {
            int temp=r[i];
            r[i]=r[z];
            r[z]=temp;
        }
    }
}
</pre>
####3.2堆排序（heap sort）

算法思路：堆有两个性质，一是堆中某个节点的值总是不大于或不小于其父节点的值，二是堆是一棵完全树。以从大到小排序为例，首先要把得到的数组构建为一个最小堆，这样父节点均是小于或者等于子结点，根节点就是最小值，然后让根节点与尾节点交换，这样一次之后，再把前n－1个元素构建出最小根堆，让根结点与第n－2个元素交换，依此类推，得到降序序列。

c语言实现代码：
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//从大到小排序
//以i节点为根，调整为堆的算法，m是节点总数，i节点的子结点为i*2+1,i*2+2
void heapMin(int r[100],int i,int m){
//    temp保存根节点，j为左孩子编号
    int j,temp;
    temp=r[i];
    j=2*i+1;

    while (j&gt;m) {
        if (j+1&lt;m &amp;&amp; r[j+1]&lt;r[j]) {//在左右孩子中找最小的
            j++;
        }
        if (r[j]&gt;=temp) {
            break;
        }

        r[i]=r[j];
        i=j;
        j=2*i+1;

    }
    r[i]=temp;

}
</pre>
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
void heapSort(int r[100],int n){
// n/2-1最后一个非叶子节点
// 下面这个操作是建立最小堆
    for (int i=n/2-1; i&gt;=0; i--) {
        heapMin(r, i, n);
    }
// 一下for语句为输出堆顶元素，调整堆操作
    for (int j=n-1; j&gt;=1; j--) {
// 堆顶与堆尾交换
        int temp=r[0];
        r[0]=r[j];
        r[j]=temp;
        heapMin(r, 0, j);
    }
//得到的就是降序序列
    for (int i=0; i&lt;n; i++) {
        printf(" %d",r[i]);
    }
}
</pre>
时间复杂度：O(n log2n)

参考网址：<a title="白话经典算法系列之七 堆与堆排序" href="http://blog.csdn.net/morewindows/article/details/6709644">白话经典算法系列之七 堆与堆排序</a>

##4 归并排序（merge sort）

####4.1两路归并排序

算法思路：它指的是将两个顺序序列合并成一个顺序序列的方法。如有数列｛6，202，100，301，38，8，1｝，第一次归并后变成了｛6，202｝，｛100，301｝，｛8，38｝，｛1｝；第二次归并后，｛6，100，202，301｝，｛1，8，38｝；第三次归并后｛1，6，8，38，100，202，301｝。

代码实现分三步，通过自底向上实现归并子算法，一趟归并扫描子算法，二路归并排序算法
<pre lang="objc" escaped="true" style="background: #E8F2FB ;">
//归并子算法
//将有序的X[s..u]和X[u+1..v]归并为有序的Z[s..v]
void merge(int X[], int Z[], int s, int u, int v)
{
    int i, j, q;
    i = s;
    j = u + 1;
    q = s;

    while( i &lt;= u &amp;&amp; j&lt;= v )
    {
        if( X[i] &lt;= X[j] )
            Z[q++] = X[i++];
        else
            Z[q++] = X[j++];
    }

    while( i &lt;= u )   //将X中剩余元素X[i..u]复制到Z
        Z[q++] = X[i++];
    while( j &lt;= v )   //将X中剩余元素X[j..v]复制到Z
        Z[q++] = X[j++];
}</pre>
<pre lang="objc" line="40" escaped="true" style="background: #E8F2FB ;">
/*
一趟归并扫描子算法
将参加排序的序列分成若干个长度为 t 的，且各自按值有序的子序列，然后多次调用归并子算法merge将所有两两相邻成对的子序列合并成若干个长度为2t 的，且各自按值有序的子序列。
若某一趟归并扫描到最后，剩下的元素个数不足两个子序列的长度时：
若剩下的元素个数大于一个子序列的长度 t 时，则再调用一次归并子算法 merge 将剩下的两个不等长的子序列合并成一个有序子序列；
若剩下的元素个数小于或者等于一个子序列的长度 t 时，只须将剩下的元素依次复制到前一个子序列后面。
*/

/* X[0..n-1]表示参加排序的初始序列
* t为某一趟归并时子序列的长度
* 整型变量i指出当前归并的两个子序列中第1个子序列的第1个元素的位置
* Y[0..n-1]表示这一趟归并后的结果
*/


void mergePass(int X[], int Y[], int n, int t)
{
    int i = 0, j;
    while( n - i &gt;= 2 * t ) //将相邻的两个长度为t的各自有序的子序列合并成一个长度为2t的子序列
    {
        merge(X, Y, i, i + t - 1, i + 2 * t - 1);
        i = i + 2 * t;
    }

    if( n - i &gt; t ) //若最后剩下的元素个数大于一个子序列的长度t时
        merge(X, Y, i, i + t - 1, n - 1);
    else //n-i &lt;= t时，相当于只是把X[i..n-1]序列中的数据赋值给Y[i..n-1]
        for( j = i ; j &lt; n ; ++j )
            Y[j] = X[j];
}
//二路归并排序算法
void mergeSort(int X[], int n)
{
    int t = 1;
    int *Y = (int *)malloc(sizeof(int) * n);
    while( t &lt; n )
    {
        mergePass(X, Y, n, t);
        t *= 2;
        mergePass(Y, X, n, t);
        t *= 2;
    }
    free(Y);
}
void print_array(int array[], int n)
{
    int i;
    for( i = 0 ; i &lt; n ; ++i )
        printf("%d ", array[i]);
    printf("\n");
}
int main()
{
    int array[] = {65, 2, 6, 1, 90, 78, 105, 67, 35, 23, 3, 88, -22};
    int size = sizeof(array) / sizeof(int);
    mergeSort(array, size);
    print_array(array, size);
    return 0;
}</pre>

时空复杂度：二路归并排序算法：将参加排序的初始序列分成长度为1的子序列使用mergePass函数进行第一趟排序，得到 n / 2 个长度为 2 的各自有序的子序列（若n为奇数，还会存在一个最后元素的子序列），再一次调用mergePass函数进行第二趟排序，得到 n / 4 个长度为 4 的各自有序的子序列， 第 i 趟排序就是两两归并长度为 2^(i-1) 的子序列得到 n / (2^i) 长度为 2^i 的子序列，直到最后只剩一个长度为n的子序列。由此看出，一共需要 log2n 趟排序，每一趟排序的时间复杂度是 O(n), 由此可知

该算法的总的时间复杂度是是 O(n log2n)，但是该算法需要 O(n) 的辅助空间，空间复杂度很大，是 O(n).

##5 基数排序（radix sort）
算法思路：

基数排序可以采用LSD（Least significant digital）或者MSD（Most significant digital），LSD的排序由键值的最右边开始，MSD从最左边开始。

以LSD为例，假设原来有一串数值如下所示：

73, 22, 93, 43, 55, 14, 28, 65, 39, 81

第一步

首先根据个位数的数值，在走访数值时将它们分配至编号0到9的桶子中：

0

1 81

2 22

3 73 93 43

4 14

5 55 65

6

7

8 28

9 39

第二步

接下来将这些桶子中的数值重新串接起来，成为以下的数列：

81, 22, 73, 93, 43, 14, 55, 65, 28, 39

接着再进行一次分配，这次是根据十位数来分配：

0

1 14

2 22 28

3 39

4 43

5 55

6 65

7 73

8 81

9 93

第三步

接下来将这些桶子中的数值重新串接起来，成为以下的数列：

14, 22, 28, 39, 43, 55, 65, 73, 81, 93

这时候整个数列已经排序完毕；如果排序的对象有三位数以上，则持续进行以上的动作直至最高位数为止。

LSD的基数排序适用于位数小的数列，如果位数多的话，使用MSD的效率会比较好。

关于各个算法的对比可以看百度百科的这张图：
<img src="http://7u2k5i.com1.z0.glb.clouddn.com/coderyi_sort.png" alt="sort" />

更多内容可以看这里：<a title="C语言常用算法讲解" href="http://c.biancheng.net/cpp/u/c14/">C语言常用算法讲解</a>
