title: SQL防注入与递归查询
date: 2015-09-07 16:37:03
tags: .Net
---

sql递归查询，防注入：

```
	 DataTable result = new DataTable();

            result.Columns.Add("id");
            result.Columns.Add("nickName");
            result.Columns.Add("parentUser");
            result.Columns.Add("price");
            result.Columns.Add("total");
            result.Columns.Add("counts");
            int userid = Convert.ToInt32((SqlHelper.ExecuteDataset(SqlConnectionString.conn, CommandType.Text, "select id from dbo.tb_user where userName= '" + (Session["admin_user"] as Models.User).UserName + "'")).Tables[0].Rows[0]["id"]);
            kw = Request.QueryString["kw"];
            GetDis(result, userid, kw);
            rptUser.DataSource = result.DefaultView;
            rptUser.DataBind();
```


```
	
        private static void GetDis(DataTable result, int userid, string key)
        {
            SqlParameter[] parm = new SqlParameter[] {
            new SqlParameter("nickName","%"+key+"%")
            };
            string sql = "select * from dbo.[View_Shop_Total] where [parentUser]=" + userid + " and nickName like @nickName ";
            var ds = SqlHelper.ExecuteDataset(SqlConnectionString.conn, CommandType.Text, sql, parm);
            if (ds.Tables.Count > 0)
            {
                var dt = ds.Tables[0];
                foreach (DataRow r in dt.Rows)
                {
                    result.Rows.Add(r.ItemArray);
                    //递归
                    var id = Convert.ToInt32(r["id"]);
                    GetDis(result, id, key);
                }
            }
        }
```