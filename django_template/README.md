# Django Template

## 來加些 HTML、CSS 吧

目前為止我們在瀏覽器上面看到的都只是純文字，感覺總不太漂亮。所以現在我們來加上 HTML 跟 CSS 來美化這個網頁吧！

## Django Template

Django 提供了 Django Template 來幫助我們可以產出 HTML 頁面。

讓我們在 excuse 目錄底下建立一個 templates 目錄。裡面新增一個 index.html 檔案，內容如下

```html
<!DOCTYPE HTML>
<html style="height: 100%">
<head>
 <title>Clone of Excuses For Lazy Coders</title>
</head>
<body style="height: 100%">
 <div class="wrapper" style="min-height: 100%; height: auto !important; height: 100%; margin: 0 auto -8em;">
  <center style="color: #333; padding-top: 200px; font-family: Courier; font-size: 24px; font-weight: bold;"><a href="/" rel="nofollow" style="text-decoration: none; color: #333;">{{ excuse }}</a></center>
  <div class="push"></div>
 </div>
 <div class="footer">
  <center style="color: #333; font-family: Courier; font-size: 11px;">
   <br /><br />&copy; Copyleft
  </center>
 </div>
 </body>
</html>
```

在這邊，我們透過了 {{ excuse }} 讓 Django View 把變數傳入 template 當中。

接著，打開我們之前寫的 excuse/views.py，把之前寫的 home function 改成：

```python
from django.shortcuts import render

def home(request):
    excuses = [
        "It was working in my head",
        "I thought I fixed that",
        "Actually, that is a feature",
        "It works on my machine",
    ]

    excuse = random.choice(excuses)
    return render(request, "index.html", {'excuse': excuse})
```

我們 import 了 django.shortcuts.render，這個 function 可以幫助我們讀取 template 之後，把變數以 python dictionary 的方式傳入 template 當中，最後再 render 出來並且回傳到瀏覽器中。接著 reload 瀏覽器，如果沒做錯的話應該可以看到跟剛剛一樣的結果！

### 小練習

看一下 [Django Template 的說明文件](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/)，如果想要把 excuse 變成全部都大寫的話該怎麼做呢？
