# 一步一步学习架构（二）
自上次设计完表之后，现今需要根据用户需求来设计，增删改查的功能，对某些表的特殊操作。

```java
//根据用户id集合查询
List<SysUser> selectByIdList(Long[] idArray);

```
批量插入用户信息
```xml
// 批量插入用户信息
int insertList(List<SysUser> userList);
//在 UserMapper.xml 中添加如下 SQL。
<insert id="insertList">
    insert into sys_user(
        user_name, user_password,user_email,
        user_info, head_img, create_time)
    values
    <foreach collection="list" item="user" separator=",">
        (
        #{user.userName}, #{user.userPassword},#{user.userEmail},
        #{user.userInfo}, #{user.headImg, jdbcType=BLOB}, #{user.createTime, jdbcType=TIMESTAMP})
    </foreach>
</insert>
```

Mapper有以下通用方法
```java
//根据实体类不为null的字段进行查询,条件全部使用=号and条件
List<T> select(T record);

//根据实体类不为null的字段查询总数,条件全部使用=号and条件
int selectCount(T record);

//根据主键进行查询,必须保证结果唯一
//单个字段做主键时,可以直接写主键的值
//联合主键时,key可以是实体类,也可以是Map
T selectByPrimaryKey(Object key);

//插入一条数据
//支持Oracle序列,UUID,类似Mysql的INDENTITY自动增长(自动回写)
//优先使用传入的参数值,参数值空时,才会使用序列、UUID,自动增长
int insert(T record);

//插入一条数据,只插入不为null的字段,不会影响有默认值的字段
//支持Oracle序列,UUID,类似Mysql的INDENTITY自动增长(自动回写)
//优先使用传入的参数值,参数值空时,才会使用序列、UUID,自动增长
int insertSelective(T record);

//根据实体类中字段不为null的条件进行删除,条件全部使用=号and条件
int delete(T key);

//通过主键进行删除,这里最多只会删除一条数据
//单个字段做主键时,可以直接写主键的值
//联合主键时,key可以是实体类,也可以是Map
int deleteByPrimaryKey(Object key);

//根据主键进行更新,这里最多只会更新一条数据
//参数为实体类
int updateByPrimaryKey(T record);

//根据主键进行更新
//只会更新不是null的数据
int updateByPrimaryKeySelective(T record);
```
现在，在我们了解了方法之后，我们需要设计对每个表的操作
首先，基础操作，增删改查一套