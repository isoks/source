title: .net sql高级分页
date: 2015-08-28 11:01:27
tags: .Net
---

sql 高级分页 

```
    #region 分页
        /// <summary>
        /// 分页
        /// </summary>
        /// <param name="index">页索引(从1开始)</param>
        /// <param name="count">每页显示数据条数</param>
        /// <param name="where">条件(abc=1)</param>
        /// <param name="order">排序(abc desc)</param>
        /// <param name="param">参数</param>
        /// <returns>实体类泛型集合</returns>
        public List<material> page(int index, int count, string where, string order, params  SqlParameter param)
        {
            index = index >= 1 ? index : 1;
            count = count >= 1 ? count : 10;
            //sql 语句
            string sql = "select * from "
            + " ( "
            + " select ROW_NUMBER() over(order by [__Happy] desc) as [__Code] ,* from "
            + " ( "
            + " select top 500000 *,'code' as [__Happy] "
            + " from dbo.[tb_material]";
            if (!string.IsNullOrEmpty(where))
            {
                sql += string.Format(" where 1=1 and {0} ", where);
            }
            if (!string.IsNullOrEmpty(order))
            {
                sql += string.Format(" order by {0} ", order);
            }
            sql += " )h "
            + " )c "
            + string.Format(" where [__Code]>({1}*({0}-1)) and [__Code]<=({1}*{0}) ; ", index, count);
            //查询
            SqlDataReader dr;
            if (param != null)
            {
                dr = SqlHelper.ExecuteReader(SqlConnectionString.conn, CommandType.Text, sql, param);
            }
            else
            {
                dr = SqlHelper.ExecuteReader(SqlConnectionString.conn, CommandType.Text, sql);
            }
            List<material> list = new List<material>();
            while (dr.Read())
            {
                material m = new material();
                m.Id = Convert.ToInt32(dr["id"]);
                m.Content = dr["content"].ToString();
                list.Add(m);
            }
            if (dr != null)
            {
                dr.Close();
            }
            return list;
        }
        #endregion
```


```
select * from(
    select  ROW_NUMBER() over(order by z) as e ,* from (
        select top 999999 'z' as z,*  from dbo.Tixian
        where Id>0
        order by Id desc
    )t 
)b
where e>(10*(2-1)) and e <=10*(2);
```



清空数据库   truncate table     
注意！有主外键关系的主表不可以 truncate table

output inserted.字段   --插入数据，并返回刚刚插入的数据id

insert into a (name) output inserted.id values('ejkhrfe');返回id 