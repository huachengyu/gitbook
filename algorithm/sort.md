#### 常见排序算法
@Date 2016.06.22

> 冒泡排序
* 时间复杂度O(n²)
* 循环遍历,位置相邻比较
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/sort/BubbleSort.java)

> 快速排序
* 基于分治模式处理
* 平均时间 : O(n log(n))
* 最坏情况 : O(n²)
* 分区操作 : 选择一个基准数,比基准小的在左边,比基准大的在右边;对于基准左右两个子集,继续进行以上操作,递归
* 缺点 : 小数组效率低,重复数据多效率低
* 不稳定排序
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/sort/QuickSort.java)