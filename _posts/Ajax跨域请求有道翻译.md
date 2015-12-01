title: Ajax跨域请求有道翻译
date: 2015-08-14 09:02:59
tags: .Net
---

**json不能够跨域，本次是jsonp跨域请求有道API .**


```
 jQuery.ajax({
                type: 'data',
                async: false,
                url: 'http://fanyi.youdao.com/openapi.do?keyfrom=ezcodor&key=1263817274&type=data&doctype=jsonp&callback=show&version=1.1&q=' + Id + '',
                dataType: 'jsonp',
                cache: true,
                contentType: 'application/x-www-form-urlencoded;charset=utf-8',
                jsonp: "callback",
                jsonpCallback: "show",
                success: function (data) {
                    t_json = jsonToString(data);
                    t_json = t_json.replace("us-", "us");
                    var json = JSON.parse(t_json);
                    console.log(json);
},
});

```

jsontostring方法在[这里](http://isoks.github.io/2015/08/13/JsonToString/)

---

- 请求有道翻译发音

``` 

  var audio = document.getElementById('hello');
            audio.src = 'http://dict.youdao.com/dictvoice?audio=hello&type=1/2';
            setTimeout(function () {
                $('#hello').attr('src', 'http://dict.youdao.com/dictvoice?audio=' + Id + '&type=1');
            }, 3000);
})

```

隐藏audio.用触发事件控制audio.play();