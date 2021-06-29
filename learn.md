* redis 的列表是 链表实现的 插入快（O(1)），查询慢（O(N)），
为什么要这么设计? 对数据库系统而言，将元素添加到很长的列表中
非常重要， 另一个优势是redis的列表可以在恒定时间以固定长度获取。
查询慢的解决方案：使用排序集。
列表的常见用法：
1.存储用户的最新更新，
2.生产者消费者模型（因为这里的列表可以作为队列）
但是如果消费者调用`RPOP`或者`LPOP`取得队列的元素，而队列中没有元素，可能需要“轮询”，浪费资源，因此使用`BRPOP`或者`BLPOP`可以有一个等待超时的时间。

* redis排序集 插入 O（log(N)）, 根据rank排序
排序集的常见用法：
1. 游戏的排行榜 显示前N名
2. 大量更新得分和位置 排序集是合适的 （因为排序集需要保持有序，更新的复杂度也不错）

* redis 位图 节约内存，查询效率高
位图的常见用法：
1. 实时分析
2. 高效存储与对象ID相关的bool信息（比如某个用户的在线状态，活跃天数...和用户的id关联 SETBIT 20190501 12500 1  time uid isOnLine）