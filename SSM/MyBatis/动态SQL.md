## 动态SQL的实现

mapper文件

```
<mapper namespace="entity.Teacher.mapper">
    <select id="getTeacher" resultType="User">
        select * from teacher 
            <where>
                <if test="name!=null">
                    name like CONCAT('%',#{name},'%')
                </if>
            </where>
    </select>
</mapper>
```
