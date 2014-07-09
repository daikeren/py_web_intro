# Django Template

## 來加些 HTML、CSS 吧

目前為止我們在瀏覽器上面看到的都只是純文字，感覺總不太漂亮。所以現在我們來加上 HTML 跟 CSS 來美化這個網頁吧！要怎麼做呢？很簡單，就是改我們的輸出的東西就好！

```python

def home(request):
    excuses = [
        "It was working in my head",
        "I thought I fixed that",
        "Actually, that is a feature",
        "It works on my machine",
    ]

    output = """
<!DOCTYPE HTML>
<html>
<head>
 <title>Clone Excuses For Lady Coders</title>
 <style type="text/css">* {margin: 0;} html, body {height: 100%;} .wrapper {min-height: 100%; height: auto !important; height: 100%; margin: 0 auto -8em;} .footer, .push {height: 8em;}</style>
</head>
<body>
 <div class="wrapper">
  <center style="color: #333; padding-top: 200px; font-family: Courier; font-size: 24px; font-weight: bold;"><a href="/" rel="nofollow" style="text-decoration: none; color: #333;">{excuse}</a></center>
  <div class="push"></div>
 </div>
 <div class="footer">
  <center style="color: #333; font-family: Courier; font-size: 11px;">
   <br /><br />&copy; Copyleft
  </center>
 </div>
 /body>
</html>
    """.format(excuse=random.choice(excuses))

    return HttpResponse(output)
```

在這邊，我們看到了新的 Python 語法出現。會覺得 output 這個變數長得很怪嗎？為什麼會用 """ 當括號呢？在 Python 當中，如果你想要一次用很長的字串，就可以直接用 """ 包起來。

接著，我們看到了裡面出現了 ```{excuse}``` 還有最後接了一個 [.format()](https://docs.python.org/3/library/string.html#string-formatting) 又是什麼呢？在這邊，我們借由在 .format() 當中傳入 excuse 變數，把字串當中的 {excuse} 代換成我們想要的字串。

這邊我們直接插入了 inline CSS 還有對應必備的 HTML tag，由於今天主要是講 Python Web，關於 HTML 跟 CSS 就不多說了。有興趣的人可以參考 [w3schools](http://www.w3schools.com/) 等網站 :) 打開 http://localhost:8000 看看，是不是看起來比較漂亮了呢？

### 小練習

請看一下 [String Formatting](https://docs.python.org/3/library/string.html#string-formatting) 相關的文件，看看除了上面的寫法之外，是不是還有別的寫法呢？

## Django Template

在前面我們透過在 view 當中使用直接輸出字串的方式來幫助我們輸出 HTML，不過這樣會有一個很大的壞處：通常來說 HTML 的部分可能是由前端工程師寫的，如果我們把 HTML 寫在 view 當中，會造成維護以及修改上面的困難，因此 Django 提供了 Django Template 來幫助我們解決這部分的問題。

讓我們在 excuse 目錄底下建立一個 templates 目錄。裡面新增一個 index.html 檔案，內容如下

```html
<!DOCTYPE HTML>
<html>
<head>
 <title>Clone Excuses For Lady Coders</title>
 <style type="text/css">* {margin: 0;} html, body {height: 100%;} .wrapper {min-height: 100%; height: auto !important; height: 100%; margin: 0 auto -8em;} .footer, .push {height: 8em;}</style>
</head>
<body>
 <div class="wrapper">
  <center style="color: #333; padding-top: 200px; font-family: Courier; font-size: 24px; font-weight: bold;"><a href="/" rel="nofollow" style="text-decoration: none; color: #333;">{{ excuse }}</a></center>
  <div class="push"></div>
 </div>
 <div class="footer">
  <center style="color: #333; font-family: Courier; font-size: 11px;">
   <br /><br />&copy; Copyleft
  </center>
 </div>
 /body>
</html>
```

在這邊，我們可以看到基本上跟原本我們用 string formatting 的內容差別不大，只是原本 {excuse} 的地方被改成了 {{ excuse }}，這部分會讓 Django View 把變數傳入 template 當中。

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
        return render(request, "home.html", {'excuse': article})
```

我們 import 了 django.shortcuts.render，這個 function 可以幫助我們讀取 template 之後，把變數以 python dictionary 的方式傳入 template 當中，最後再 render 出來並且回傳到瀏覽器中。接著 reload 瀏覽器，如果沒做錯的話應該可以看到跟剛剛一樣的結果！
