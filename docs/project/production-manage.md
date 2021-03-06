# 货运管理
业务：购销合同
购销合同：包括主信息，多个货物信息，多个附件信息。一个购销合同包含多个货物信息，一个货物信息包括多个附件信息。**两层一对多**

## 图片处理
> **数据一般不能出现大字段，这样子不管怎么优化索引都会无效。所以我们一般存取图片的路径，而不是整张图片。**

## 分析设计：
1. 表
	根据需求中出现的**名词**设计表。
2. 每个表的每个字段
	用户给的资料中，变化的内容，设计**字段**。
	所以，静态资源一般不出现。
	当然可以对某些静态资源设置，提升扩充性。
	用户说唯一的不能作为主键，然后我们自己设计**主键**。
3. 表之间的关系
	合同，货物----一对多
	货物，附件----一对多
4. 复杂的业务逻辑

5. 数据库表的冗余

### 面试
#### 在oracle中number(5,2)
整数2位，小数2位
5=2+1+2,1是小数点
#### 数据库设计三范式
1. 表必须有主键
2. 字段内容不能是其他内容加工而成
3. 行数据不能相同
#### 现今主流原则（某些场景下）：反三泛式
1. 中间表**无主键**
2. 字段存一些加工后的**中间结果**
3. 冗余设计，追求查询数据
冗余可能导致出处不唯一，导致增删改查量增大
> 所以设计原则是，根据三泛式，进行局部优化。

创建数据库的时候我们一般不先创建外键，因为会不好测试
同时，我们也不创建备注信息

需求调研->设计->开发->测试->试运行->正式上线-> 维护

## 数据库的备份和恢复
1. oracle pl/sql
2. mysql 自己查