title: EasyUI combobox绑定与触发事件
date: 2015-08-16 17:52:46
tags: .Net
---

**easyui的combobox 请求ashx ,触发事件，根据combobox点击列表请求ashx相应的数据 ：**

```
	 $('#CompanyName').combobox({
                url: 'ashx/StuCompanyManagementHandler.ashx?type=GetCompanyName',
                valueField: 'id',
                textField: 'text',
                editable: false,
                onChange: function (newValue, oldValue) {
                    var val = $('#CompanyName').combobox('getValue');
                    $.post('ashx/StuCompanyManagementHandler.ashx?type=GetCompanyInfo&CompanyName=' + encodeURI(val), function (msg) {
                        var json = eval("(" + msg + ")");
                        for (var i = 0; i < json.rows.length; i++) {
                            if (json.rows[i].CompanyName == val
                             {
                         $('#CompanyTel').val(json.rows[i].CompanyTel);
                                $('#CompanyWork').combobox({
                                    url: 'ashx/StuCompanyManagementHandler.ashx?type=GetCompanyWork&CompanyName=' + encodeURI(val),
                                    valueField: 'id',
                                    textField: 'text',
                                    editable: false,
                                    onChange: function (newValue, oldValue) {
                                        var val2 = $('#CompanyWork').combobox('getValue');

                                        $.post('ashx/StuCompanyManagementHandler.ashx?type=GetCompanyInfo&CompanyWork=' + encodeURI(val2), function (msg) {
                                            var json = eval("(" + msg + ")");

                                            for (var i = 0; i < json.rows.length; i++) {

                                                if (json.rows[i].CompanyWork == val2) {
                                                    $('#CompanyTeaName').val(json.rows[i].CompanyTeaName);

                                                }

                                            }

                                        });

                                    }

                                }); //end

                            } //if

                        } //for

                    });

                }

            });
	
```

**后台ashx :**

#### 1. StuCompanyManagementHandler.ashx?type=GetCompanyInfo 
- combobox json 格式：

```
	 sb.Append("[");
                    int count = dt.Rows.Count;

                    for (int i = 0; i < dt.Rows.Count; i++)
                    {

                        sb.Append("{\"id\":\"" + dt.Rows[i]["CompanyName"] + "\",\"text\":\"" + dt.Rows[i]["CompanyName"] + "\"},");

                    }
                    if (count > 0)
                    {
                        sb.Remove(sb.Length - 1, 1);
                    }
                    sb.Append("]");
                }
                context.Response.Write(sb.ToString());
```


```
	  if (context.Request.QueryString["type"] == "GetCompanyWork")
        {
            string CompanyName = context.Request.QueryString["CompanyName"].ToString();
            try
            {
                System.Data.DataTable dt = new System.Data.DataTable();
                string SQL = "select distinct CompanyWork from SPWCompanyDetails where CompanyName='" + CompanyName + "'";
                dt = Utility.DBHelper.GetDT(SQL);
                System.Text.StringBuilder sb = new System.Text.StringBuilder();
                if (dt.Rows.Count == 0)
                {
                    sb.Append("{[]}");
                }
                else
                {
                    sb.Append("[");
                    int count = dt.Rows.Count;

                    for (int i = 0; i < dt.Rows.Count; i++)
                    {

                        sb.Append("{\"id\":\"" + dt.Rows[i]["CompanyWork"] + "\",\"text\":\"" + dt.Rows[i]["CompanyWork"] + "\"},");

                    }
                    if (count > 0)
                    {
                        sb.Remove(sb.Length - 1, 1);
                    }
                    sb.Append("]");
                }
                context.Response.Write(sb.ToString());
            }
            catch (Exception e)
            {

            }
        } 
```

