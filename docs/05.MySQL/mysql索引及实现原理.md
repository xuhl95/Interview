# MySQL索引及其实现原理(基于MyISAM及InnoDB引擎)

![索引分类](./assets/index.png)

##目录
- 数据结构及算法基础
   - 索引的本质
   - B Tree和B+Tree
   - 为什么使用B Tree（B+Tree）
   - 主存存取原理
- MySQL索引实现
   - MyISAM索引实现
   - InnoDB索引实现
- 索引使用策略及优化
   - 索引的好处
   - 什么情况下可以用到B树索引
   - 示例数据库
   - 最左前缀原理与相关优化
   - Btree索引的使用限制
   - 什么情况下设置了索引但无法使用
   - 索引选择性与前缀索引
   - 索引并不是越多越好。下列情况下不建议建索引
   - InnoDB的主键选择与插入优化
- Hash索引的特点
- Hash索引的限制

参考文章:
 
 [MySQL索引及其实现原理(基于MyISAM及InnoDB引擎)](https://cloud.tencent.com/developer/article/1125452)
 
 [MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)