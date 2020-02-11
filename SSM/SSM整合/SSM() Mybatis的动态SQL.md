# Mybatis的动态SQL

Mybatis的动态SQL是基于OGNL表达式的，可以方便的在SQL语句中实现某些逻辑，总体说来Mybatis动态SQL语句主要有以下几类：

- if语句（简单的条件判断）
- choose（when，otherwise），相当于java中的switch，与jstl中的choose类似
- trim（对包含类容加上前缀，后缀等）
- where（主要用来简化sql语句中where条件判断，能智能处理and，or，不必担心多余导致语法错误）
- set（用于更新时）
- foreach（可用于批量进行的操作）

## if

```
<select id="dynamicIfTest" parameterType="Blog" resultType="Blog">
  select * from t_blog where 1 = 1
  <if test="title != null">
    and title = #{title}
  </if>
  <if test="content != null">
    and content = #{content}
  </if>
  <if test="owner != null">
    and owner = #{owner}
  </if>
</select>
```

- **test**是判断条件，如果满足条件，那么就会在SQL语句上拼接上<if>标签中的类容

----

## choose

```
<select id="dynamicChooseTest" parameterType="Blog" resultType="Blog">
select * from t_blog where 1 = 1
  <choose>
    <when test="title != null">
      and title = #{title}
    </when>
    <when test="content != null">
      and content = #{content}
    </when>
    <otherwise>
      and owner = "owner1"
    </otherwise>
  </choose>
</select>
```
- **when**表示：当when中的条件满足的时候就输出其中的内容，当 when 中有条件满足的时候，就会跳出 choose
- **otherwise**表示：当所有的条件都不满足的时候就输出 otherwise 中的内容

----

## trim

```
<select id="dynamicTrimTest" parameterType="Blog" resultType="Blog">
select * from t_blog
  <trim prefix="where" prefixOverrides="and |or">
    <if test="title != null">
      title = #{title}
    </if>
    <if test="content != null">
      and content = #{content}
    </if>
    <if test="owner != null">
      or owner = #{owner}
    </if>
  </trim>
</select>
```
- **prefix**：在trim标签包裹的部分的前面添加内容
- **suffix**：在trim标签包裹的部分的后面添加内容
- **prefixOverrides**：找寻子句首部的命中词（prefixOverrides的值），如果找到，则删除该词，如果没找到就算了，命中词之间以 | 分隔
  - AND 和 OR后面的空格不能省略，这是为了避免匹配到andes、orders等单词
  - prefixOverriders包含的值除了AND和OR之外，还有：**AND\n**,**OR\n*,**AND\r**,**OR\r**,**AND\t**,**OR\t**
- **suffixOverrides**：找寻子句尾部的命中词（suffixOverrides的值），如果找到，则删除该词，如果没找到就算了，命中词之间以 | 分隔

----

## where

```
<select id="dynamicWhereTest" parameterType="Blog" resultType="Blog">
  select * from t_blog
  <where>
    <if test="title != null">
      title = #{title}
    </if>
    <if test="content != null">
      and content = #{content}
    </if>
    <if test="owner != null">
      and owner = #{owner}
    </if>
  </where>
</select>
```

- **where**元素的作用是会在写入 where 元素的地方输出一个 where，
- 另外一个好处是你不需要考虑 where 元素里面的条件输出是什么样子的，MyBatis 会智能的帮你处理，
- 如果所有的条件都不满足，那么SQL语句中就不会有where，MyBatis 就会查出所有的记录，
- 如果输出后是 and 开头的，MyBatis 会把第一个 and 忽略，当然如果是 or 开头的，MyBatis 也会把它忽略；
- 此外，在 where 元素中你不需要考虑空格的问题，MyBatis 会智能的帮你加上

----

## set

```
<update id="dynamicSetTest" parameterType="Blog">
  update t_blog
  <set>
    <if test="title != null">
      title = #{title},
    </if>
    <if test="content != null">
      content = #{content},
    </if>
    <if test="owner != null">
      owner = #{owner}
    </if>
  </set>
  where id = #{id}
</update>
```

- set 元素主要是用在更新操作的时候，它的主要功能和 where 元素其实是差不多的，主要是在包含的语句前输出一个set
- 如果包含的语句是以逗号结束的话将会把该逗号忽略，
- 如果 set 包含的内容为空的话则会出错，在SQL语句中也不会有set

----

## foreach

foreach可以在 SQL 语句中进行迭代一个集合</br>
foreach 元素的属性主要有 :
  - **collection**：必填，迭代循环的属性名，如果是list集合，则值为list，如果是数组，则值为array，如果是map基哥
  - **item**：item 表示集合中每一个元素进行迭代时的别名，在进行迭代的时候，需要使用**item.**来获取item的各个属性
  - **index**：index是索引名，在Iterable类型下为当前索引值，当Map类型下为Map的Key
  - **open**：整个循环内容开头的字符串
  - **close**：整个循环内容结尾的字符串
  - **separator**：每次循环的分隔符



### 单参数List类型

```
<select id="dynamicForeachTest" resultType="Blog">
  select * from t_blog where id in
  <foreach collection="list" index="index" item="item" open="(" separator="," close=")">
    #{item}
  </foreach>
</select>
```

### 单参数数组类型

```
<select id="dynamicForeach2Test" resultType="Blog">
  select * from t_blog where id in
  <foreach collection="array" index="index" item="item" open="(" separator="," close=")">
    #{item}
  </foreach>
</select>
```

### Map类型参数

```
<select id="dynamicForeach3Test" resultType="Blog">
  select * from t_blog where title like "%"#{title}"%" and id in
  <foreach collection="ids" index="index" item="item" open="(" separator="," close=")">
    #{item}
  </foreach>
</select>
```

