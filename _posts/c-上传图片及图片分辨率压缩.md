title: 'c#上传图片及图片压缩'
date: 2015-09-28 11:11:03
tags:  .Net
---

c# 上传图片。

---

图片压缩类：[ThumbnailImage](http://7xl2ye.com1.z0.glb.clouddn.com/ThumbnailImage.rar)


.aspx:
```
	    <form method="get" id="formAddAdmin" class="form-horizontal">
                        <div class="form-group">
                            <div style="margin-left:13px;" class="col-sm-10">
                                <img src="<%=ws.Images %>" id="imgs" style="width: 100px; height: 100px;" />
                                <input type="hidden" name="Images" value="<%=ws.Images %>" />
                                <input type="file" name="inputImg" id="inputImg" style="display: initial" />
                                <button class="btn btn-primary btn-xs" id="uploadFile" type="button">上传</button>
                            </div>
                        </div>
                    </form>

 <div class="modal-footer">
                    <button type="button" class="btn btn-white btn-sm" data-dismiss="modal">关闭</button>
                    <button type="button" id="btnAdd" class="btn btn-primary btn-sm">保存</button>
                </div>
```

```
 $("#uploadFile").click(function () {
           
                $.ajaxFileUpload({
                    type: "GET",
                    url: "../ashx/FileUploadHandler.ashx",
                    fileElementId: 'inputImg',
                    dataType: "text",
                    success: function (data) {
                        $("#imgs").attr("src", data);
                        $("input[name=Images]").val(data);
                    },
                });
            });
```

**FileUploadHandler.ashx :**
```
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Web;

namespace RS.UI.ashx
{
    /// <summary>
    /// FileUploadHandler 的摘要说明
    /// </summary>
    public class FileUploadHandler : IHttpHandler
    {

        public void ProcessRequest(HttpContext context)
        {

            if (context.Request.Files.Count > 0)
            {
                var file = context.Request.Files[0];
                //图片服务器
                //string[] urls = new string[]{
                //    };
                //选择一个服务器
                //Random r = new Random();
                //string url = urls[r.Next(urls.Length)];
                //获取文件拓展名
                var nameEx = Path.GetExtension(file.FileName);
                if (CheckImageExt(nameEx))
                {
                    //生成图片名称
                    string _name = GetImageName();
                    string name = _name + nameEx;
                    string filename = "uploadImg";
                    string path = "../" + filename + "/" + name;
                    HttpPostedFile f = file;
                    f.SaveAs(System.Web.HttpContext.Current.Server.MapPath(path));

//图片压缩
//
 ThumbnailImage ti = new ThumbnailImage();
                    ti.GetThumbnail(System.Web.HttpContext.Current.Server.MapPath(path), System.Web.HttpContext.Current.Server.MapPath(spath), 384, 640, 1, Color.White);


//
                    context.Response.Write(path);
                }
                else
                {
                    context.Response.Write("图片上传失败。");
                }
            }
        }

        private byte[] StreamToByte(Stream stream)
        {
            byte[] bt = new byte[stream.Length];
            //读取
            stream.Read(bt, 0, bt.Length);
            //设置为开始
            stream.Seek(0, SeekOrigin.Begin);
            return bt;
        }
        public static bool CheckImageExt(string _ImageExt)
        {
            string[] allowExt = new string[] { ".gif", ".jpg", ".jpeg", ".bmp", ".png" };
            for (int i = 0; i < allowExt.Length; i++)
            {
                if (string.Compare(allowExt[i], _ImageExt, true) == 0)
                { return true; }
            }
            return false;

        }

        private static string GetImageName()
        {
            Random rd = new Random();
            StringBuilder serial = new StringBuilder();
            serial.Append(DateTime.Now.ToString("yyyyMMddHHmmssffff"));
            serial.Append(rd.Next(0, 999999).ToString());
            return serial.ToString();

        }
        private static bool GetPath(string strPath)
        {
            if (Directory.Exists(System.Web.HttpContext.Current.Server.MapPath(strPath)) == false)
            {
                Directory.CreateDirectory(System.Web.HttpContext.Current.Server.MapPath(strPath));
            }
            return true;
        }

        public bool IsReusable
        {
            get
            {
                return false;
            }
        }
    }
}
```


```
  $("#btnAdd").click(function () {
                $.ajax({
                    type: "post", //以post方式与后台沟通 
                    url: "../ashx/PictureCarouselHandler.ashx?type=add", //与此页面沟通 
                    dataType: 'text',//返回的值以 JSON方式 解释 
                    data: { "Addpic": "" + GetJson("#formAddAdmin") + "" },
                    success: function (judge) {
                        if (judge == "True") {
                            $("#addAdminModal").modal("hide");
                            alert("添加成功！");
                            t.fnReloadAjax('../ashx/PictureCarouselHandler.ashx?type=get');//刷新
                        } else {
                            alert("添加失败！");
                        }
                    }
                });
            });
```

**PictureCarouselHandler.ashx:**
```
	insert .......
```