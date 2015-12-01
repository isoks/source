title: .net备份sql数据库
date: 2015-09-14 12:07:26
tags: .Net
---

备份与打包SQL数据库 

如图
<img src="http://7xl2ye.com1.z0.glb.clouddn.com/121.png" />


####web.config :

```
	  <appSettings>
    <add key="SqlConnectionString" value="Data Source=XXX.XXX.XXX.XX;Initial Catalog=XX;User ID=sa;PWD=sa"/>
  </appSettings>
```

#### DAL :

```
	#region 备份数据库数据访问类
/// <summary>
/// 备份数据库数据访问类
/// </summary>
public class DataBackService
{
    /// <summary>
    /// 备份数据库方法
    /// </summary>
    /// <returns></returns>
    public string DataBack(string DataBaseName)
    {

        string returnStr = "";
        string FullPath = @"D:\ShopBestBaseBackup\" + DateTime.Now.Day.ToString() + DateTime.Now.Hour.ToString() + DateTime.Now.Minute + @"\" + DataBaseName + "";

        if (CreateFolder(FullPath))
        {
            //try
            {

                string CommandText = "backup database " + DataBaseName + " to disk='" + FullPath + "\\" + DataBaseName + ".bak'";
                SqlConnection Connection = new SqlConnection(SqlConnectionString.conn);
                Connection.Open();
                SqlCommand command = new SqlCommand(CommandText, Connection);
                command.ExecuteNonQuery();
                Connection.Close();

            }

        }
        return returnStr;
    }
    /// <summary>
    /// 创建存储路径方法
    /// </summary>
    /// <param name="Path"></param>
    /// <returns></returns>
    private bool CreateFolder(string Path)
    {
        if (!Directory.Exists(Path))
            Directory.CreateDirectory(Path);

        if (Directory.Exists(Path))
            return true;
        else
            return false;
    }

    /// <summary>
    /// 打包数据库
    /// </summary>
    /// <param name="DataBastName"></param>
    /// <param name="BackFiled"></param>
    /// <returns></returns>
    public string DataPack(string DataBastName, string BackFiled)
    {
        string returnStr = "";
        string tmp = "";
        RunProcess run = new RunProcess();
        string arcfile = "D:\\ShopBestBaseBackup\\" + DateTime.Now.Day.ToString() + DateTime.Now.Hour.ToString() + DateTime.Now.Minute + "\\shop\\" + DataBastName + ".bak";
        tmp = "a " + BackFiled + " " + arcfile;
        run.Run("D:\\WRAR\\WinRAR压缩软件\\WinRAR.exe", tmp);
        if (run.HasError)
        {
            returnStr = run.Output + "-FAILED!";
        }
        else
        {
            returnStr = "打包数据库成功！";

        }
        return returnStr;
    }
}
#region 打包数据库辅助类
class ReadErrorThread
{
    System.Threading.Thread m_Thread;
    System.Diagnostics.Process m_Process;
    String m_Error;
    bool m_HasExisted;
    object m_LockObj = new object();

    public String Error
    {
        get
        {
            return m_Error;
        }
    }

    public bool HasExisted
    {
        get
        {
            lock (m_LockObj)
            {
                return m_HasExisted;
            }
        }

        set
        {
            lock (m_LockObj)
            {
                m_HasExisted = value;
            }
        }
    }

    private void ReadError()
    {
        StringBuilder strError = new StringBuilder();
        while (!m_Process.HasExited)
        {
            strError.Append(m_Process.StandardError.ReadLine());
        }

        strError.Append(m_Process.StandardError.ReadToEnd());

        m_Error = strError.ToString();
        HasExisted = true;
    }

    public ReadErrorThread(System.Diagnostics.Process p)
    {
        HasExisted = false;
        m_Error = "";
        m_Process = p;
        m_Thread = new Thread(new ThreadStart(ReadError));
        m_Thread.Start();
    }

}
class RunProcess
{
    private String m_Error;
    private String m_Output;

    public String Error
    {
        get
        {
            return m_Error;
        }
    }

    public String Output
    {
        get
        {
            return m_Output;
        }
    }

    public bool HasError
    {
        get
        {
            return m_Error != "" && m_Error != null;
        }
    }

    public void Run(String fileName, String para)
    {
        StringBuilder outputStr = new StringBuilder();

        //try
        {

            Microsoft.Win32.RegistryKey key;
            key = Microsoft.Win32.Registry.LocalMachine.OpenSubKey(@"software\microsoft\PCHealth\ErrorReporting\", true);
            int doReport = (int)key.GetValue("DoReport");

            if (doReport != 0)
            {
                key.SetValue("DoReport", 0);
            }

            int showUI = (int)key.GetValue("ShowUI");
            if (showUI != 0)
            {
                key.SetValue("ShowUI", 0);
            }
        }
        //catch { }


        m_Error = "";
        m_Output = "";
        //try
        {
            System.Diagnostics.Process p = new System.Diagnostics.Process();

            p.StartInfo.FileName = fileName;
            p.StartInfo.Arguments = para;
            p.StartInfo.UseShellExecute = false;
            p.StartInfo.RedirectStandardInput = true;
            p.StartInfo.RedirectStandardOutput = true;
            p.StartInfo.RedirectStandardError = true;
            p.StartInfo.CreateNoWindow = true;

            p.Start();

            ReadErrorThread readErrorThread = new ReadErrorThread(p);

            while (!p.HasExited)
            {
                outputStr.Append(p.StandardOutput.ReadLine() + "\r\n");
            }

            outputStr.Append(p.StandardOutput.ReadToEnd());

            while (!readErrorThread.HasExisted)
            {
                Thread.Sleep(1);
            }

            m_Error = readErrorThread.Error;
            m_Output = outputStr.ToString();
        }
        //catch (Exception e)
        //{
        //    m_Error = e.Message;
        //}
    }

}
#endregion
#endregion
```


#### BLL : 

```
	 #region 备份数据库业务逻辑类
    /// <summary>
    /// 备份数据库业务逻辑类
    /// </summary>
    public class DataBackManager
    {
        protected DataBackService db = new DataBackService();
        /// <summary>
        /// 备份数据库
        /// </summary>
        /// <returns></returns>
        public string DataBack(string DataName)
        {
            return db.DataBack(DataName);
        }
        /// <summary>
        /// 打包备份数据库
        /// </summary>
        /// <param name="DataName"></param>
        /// <param name="DataFiled"></param>
        /// <returns></returns>
        public string DataPack(string DataName, string DataFiled)
        {
            return db.DataPack(DataName, DataFiled);
        }
    }
    #endregion
```

####备份数据库触发事件：

```
	if (this.TextBox1.Text.Length < 1)
        {
            this.TextBox1.Style.Add("color", "red");
            this.TextBox1.Text = "请输入数据库名称！";
        }
        else
        {
            string Message = db.DataBack(this.TextBox1.Text.Trim());
            this.TextBox1.Text = Message;
            this.TextBox1.Style.Add("color", "green");
            this.TextBox1.Text = "数据库备份成功！";
        }
```

#### 打包数据库触发事件:

```
	 string Message = db.DataPack(this.txtDataName.Text.Trim(), this.txtfiledUrl.Text.Trim());
            if (Message.IndexOf("成功") < 1)
                this.lblMessage.Style.Add("color", "red");
            else
                this.lblMessage.Style.Add("color", "green");
            this.lblMessage.Text = Message;
```