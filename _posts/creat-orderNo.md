title: creat orderNo.
date: 2015-11-30 13:28:50
tags:  .Net
---

创建订单号：

```
	private static readonly object key = new object();
		private string buidOrderNo()
		{
			lock (key)
			{
				string number;
				bool flag;
				buidOrderNo(out number, out flag);
				while (flag)
				{
					buidOrderNo(out number, out flag);
				}
				return number;
			}
		}
		private static void buidOrderNo(out string number, out bool flag)
		{
			Random r = new Random();
			number = "PM_" + DateTime.Now.ToString("yyyyMMddHHmmssffff") + r.Next(9) + r.Next(9) + r.Next(9);
			flag = tb_orderBll.Instance.Get("orderNo='" + number + "'").Count > 0;
		}
```