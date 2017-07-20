# hive-note

## 动态partition
```
hive.exec.dynamic.partition --开启动态partition
hive.exec.max.dynamic.partitions --默认(1000) 5000
hive.exec.max.dynamic.partitions.pernode --默认(100) 2000
```
## hive的优化

### Map
```
num_map_tasks = max[${mapred.min.split.zise}, min[${dfs.block.size HDFS},${mapred.max.split.size}]]
```
```
mapred.min.split.size --数据的最小分割单元大小,默认1B
mapred.max.split.size --数据的最大分割单元大小,默认256MB
dfs.block.size --HDFS设置的数据块大小
```
### Reduce
```
num_reduce_tasks = min[${hive.exec.reducers.max},(${input.size}/${hive.exec.reducers.bytes.per.reducer})]
```
```
hive.exec.reducers.max --最大reducer个数,默认999(1007)
hive.exec.reducers.bytes.per.reducer --每个reducer的大小,默认1G(67M)
```
```
mapred.reduce.tasks --直接指定reduce个数
```
优化时找到合适的reduce个数，不是越多越好，有多少个reduce就会有多少个输出文件

## MR之间优化
### Spill Sort
```
io.sort.mb
io.sort.factor
```
### Copy
```
mapred.reduce.slowstart.completed.maps --从map端copy到reduce端,reduce在map完成默认5%的情况下启动
tasktracker.http.threads --可以决定server端的map用于提供数据传输服务的线程,默认40
mapred.reduce.parallel.copies --可以决定作为client端的reduce同时从map端拉取数据的并行度,默认30
```
### job执行模式
```
hive.exec.mode.local.auto --开启local mr模式
hive.exec.mode.local.auto.tasks.max --默认4
hive.exec.mode.local.auto.inputbytes.max --默认256
```
### JVM重用
```
mapred.job.reuse.jvm.num.tasks --设置jvm重用的次数，减少启动时间
```
### 数据倾斜
| 关键词          | 情形                                 | 后果 |
|:--------------:| ----------------------------------- | ---—- |
| join           | 其中一个是小表，key值比较集中 | 分发到某个或多个reduce上的数据远高于平均值 |
| join           | 大表与大表关联，1.key值中空值或特殊值较多; 2.key值都是有效，某个Key值量比较大; | 一个reduce处理数据量过大，非常慢 |
| group by       | groupby某些key值数据量过多 | 处理某些值的reduce非常耗时 |
| count distinct | groupby某些key值数据量过多 | 某个reduce处理数据量过大，非常耗时

1. 大小表join，优化方案：
启用mapjoin,并定义小表的文件大小不能超过250M
```
set hive.auto.convert.join = ture
set hive.mapjoin.smalltable.filessize = 2500000
```
2. 大大表join,优化方案：
    1. 通过随机函数处理空值或特殊值
    ```
    case when key is null then cast(rand(2017) as int)
         else key end
    ```
    2. 如果key值都是有效的，可以通过参数1设置每个reduce处理的数据大小，或者通过参数2设置reduce任务数。
    ```
    set hive.exec.reducers.bytes.per.reducer = 1000000000
    ```
    ```
    set mapred.reduce.tasks = 800
    ```
3. groupby的数据倾斜，优化方案：
启用map端combiner，假如map各条数据基本上不一样，combiner没有什么意义，可以设置先取100000条进行聚合，如果聚合后的数据量/100000 = 0.5则不进行combiner;
或者通过设置参数2控制生成2个MR job,第一个MR Job Map的输出结果随机分配到reduce做次预汇总,减少某些key值条数过多某些key条数过小造成的数据倾斜问题.
```
set hive.map.aggr = true
set hive.groupby.mapaggr.checkinterval = 100000（默认）
set hive.map.aggr.hash.min.reduction = 0.5（默认）
```
```
set hive.groupby.skewindata=true
```
