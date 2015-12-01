title: URL传值与接收
date: 2015-08-14 09:13:16
tags: .Net
---

Html页面：

``` 

function results() {
            var s =document.getElementById('word').value; 
            window.location.href = "Result.aspx?" + "word=" + s;
        }

``` 

后台js:

``` 

 var loc = decodeURI(location.href);
            var n1 = loc.length;
            var n2 = loc.indexOf("=");
            var Id = loc.substr(n2 + 1, n1 - n2);
或者 Request.QueryString[word]

``` 

获取Id ..



**2. **
---

```
context.Request["email"].ToString();  //获取

```