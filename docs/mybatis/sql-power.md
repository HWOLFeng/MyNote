# 04-01 SQL的力量
后端：
从表：
1. 和主表在mapper有什么不同，按照普通属性来配置

前端：
货物的外键：
1. 合同，通过get的方式在url后面加参数（主从）
2. 生产厂家，通过下拉列表的方式（只需要少量id时）
3. 面向对象获得外键，一般需要多个字段

## 04-02 修改
Controller中两个方法
转向修改页面
修改保存
前端页面
返回，和原来不同，返回的路径写绝对路径

## 04-02 删除
删除的两个方法
删除单条
删除多条

## 04-03 工具类
企业中封装的工具类
有用的上的工具类才去看，很多方法可能都没有用


## 04-04 附件表的映射文件：实体+resultMapper

## 04-05 Dao->Service（接口和实现类）->Controller（接口和实现类）

Controller转向新增页面
每个方法对应一个路径，然后每个方法内，对前面自动注入的service的方法自动调用。以及对页面参数、路径传值（action）的调用。
或者重定向，或者页面。

> Service注入dao
> Controller中注入Service

### 数据字典--跟业务无关 
一般他由：编号+名称
如：
性别：0101 男，0102女
包装单位：0201 PCS， 0202 SETS
通用的数据字典可以存放多个分类，也就是多个表的字段，必须是ID+名称的层次结构，或者是TYPE+ID+NAME的层次结构。

## 04-06 级联删除
两层一对多结构
Mybatis子表不设置外键关联时，就变成僵尸数据
Hibernate ： 多层关联 最简单：配置关系时，只关系上级对象
Mybatis 多层关联关系时，级联删除必须一级一级删除。多级时必须一级一级删除。而且还必须单独写方法。

自己去查。。。delete  * from table where 列名 in 值（可以有很多）

删除一条主表记录，同时删除子表下的批量记录。

> 反正Mybatis就是原生。是面向结果集，所以用多少写多少，别全部映射

```
示例
...
delete from product_c
where id in
<foreach collection="array" item="id" open="(" close=")" separator=",">
#{id}
</foreach>
...
```

## 04-07 三层删除
 嵌套循环语句删除----级联删除
但是还是需要每层都要删除一次，虽然是sql的高速删除

```
select count(*) 
from t_a
where column_a 
in(
select column_a
from t_b
where 条件
)
```

面试：PO、VO、BO
PO持久化对象，直接对应表
VO视图对象，一般对应页面的jsp
BO业务对象，一般对应复杂业务的时候
所以多出来的字段，需要在xml文件中体现，但是不需要持久化，这种字段称作**虚拟字段**----重点：**第32视频，04-10**

> mybatis会根据用户定义的类中的属性，将数据库中的字段类型直接强转。

## 04-11 总金额
货物新增时，同步增加总金额，在附件新增时，同理，同步计算附件总金额（由程序形成，修改删除也是一样。）

1. 用sql实现 。
货物的总金额
```
select column_a*column_b as total  
from t_a
where a_id = 'sdfsdfs'
```
附件的总金额
```
select column_a*column_b as total 
from t_a
where a_id = 'sdfsdfs'
```

>oracle可以将null值设置成别的值

## 04-12 Mybatis配置数据表关系----写collection,resultMap的继承--xml
配置VO对象，与model的区别就是需要配置对象----与之有对应关系的表。
一对多，多对多，都是需要创建VO对象。。。
然后利用新的resultMap将VO对象关联起来，**仔细模仿一下**。
一对多时，collection,ofType
多对一，association,javaType

## 04-13构建VO-SQL
合同，货物，附件，生产厂家构建复杂多级关联的SQL原则：
1. 挑选最小的结果集
2.  一层一层往上找，根据左/右链接，也称作滚雪球
ps.一般不用内链接
3.  重复字段需要取别名

```sql 
//挑字段
select a.column_a,a.column_b,a.column_c,
b.column_a,b.column_b,b.column_c
from
//左链接
(select column_a,column_b,column_c...
from table_a)a
left join
(select column_a,column_b,column_c...
from table_b)b
on a.column_a=b.column_a
```

> 级联时，通过VO对象查询数据库中的数据


