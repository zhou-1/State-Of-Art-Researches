Everything about LevelDB.      
an open-source on-disk key-value store written by Google fellows Jeffrey Dean and Sanjay Ghemawat.      

![levelDB](levelDB.png)    

具有很高的随机写，顺序读/写性能，但是随机读的性能很一般，也就是说，LevelDB很适合应用在查询较少，而写很多的场景。

leveldb提供的是KV格式的存储，支持二进制数据    
https://www.jianshu.com/p/e904834932a1    

特点：   
1、key和value都是任意长度的字节数组；   
2、entry（即一条K-V记录）默认是按照key的字典顺序存储的，当然开发者也可以重载这个排序函数；   
3、提供的基本操作接口：Put()、Delete()、Get()、Batch()；    
4、支持批量操作以原子操作进行；   
5、可以创建数据全景的snapshot(快照)，并允许在快照中查找数据；   
6、可以通过前向（或后向）迭代器遍历数据（迭代器会隐含的创建一个snapshot）；    
7、自动使用Snappy压缩数据；   
8、可移植性；    

限制：   
1、非关系型数据模型（NoSQL），不支持sql语句，也不支持索引；    
2、一次只允许一个进程访问一个特定的数据库；   
3、没有内置的C/S架构，但开发者可以使用LevelDB库自己封装一个server；   
