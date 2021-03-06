# Django URLs & Django Views

到這裡大家應該會在想 "搞了半天都在弄這些有的沒的，什麼時候才能開始做我們第一個頁面呢？" 這個章節會讓我們創立第一個頁面，並且讓我們了解到 Django 很重要的兩個部分 - URL 跟 Views.

## Django Views

一個網頁程式當中的邏輯通常是這個樣子的：request 進來 -> 從資料庫撈資料 -> 處理資料 -> 把網頁呈現出來。由於現在我們還沒有講到要怎麼樣做跟資料庫之間的互動，所以我們先來講講從 request 進來到頁面呈現出來這段過程 Django 是怎麼做的。

讓我們打開 excuse/views.py 這個檔案，輸入以下的程式碼：

```python
from django.http import HttpResponse

def home(request):
    excuses = [
        "It was working in my head",
        "I thought I fixed that",
        "Actually, that is a feature",
        "It works on my machine",
    ]
    return HttpResponse(excuses[0])
```

這是一個最簡單的 Django View，我們可以看到一個 Django View 基本上就是一個 python function，傳入值是 request，它的型別是 [HttpRequest](https://docs.djangoproject.com/en/1.9/ref/request-response/#httprequest-objects)，當中 Django 幫我們封裝了有關一個 HTTP Request 進來會傳入的資料。包含了 request body, request method... 等等。

接著，我們用到了 [Python 的 list](https://docs.python.org/3/tutorial/datastructures.html)。如果你有其他程式語言的經驗，你可以把 list 看成跟 array 很像的概念。我們宣告了一個叫做 excuses 的 list，當中放了四個字串，分別是所有的 excuses。最後，回傳一個用 excuses[0] 當 constructor 的 HttpResponse 物件。同樣的，HttpResponse 也幫我們封裝了回傳給 browser 的資訊。包含了 content type, status code 等等。

透過 Django 的 Request 還有 Response 物件，我們在處理複雜的 HTTP 的時候可以省下更多的麻煩事要處理，讓我們可以更專注在把東西做好上面。

## Django URLs

我們寫好第一個 view 之後，那麼我們該怎麼讓 Django 知道連到哪個 URL 會呼叫這個 view 呢？這就是 Django URLs 會處理的事情。讓我們打開 mysite/urls.py 這個檔案，在 urlpatterns 當中加入以下的 code

```python
urlpatterns = [
    ...
    url(r'^$', 'excuse.views.home'),
]
```

在這邊，我們可以看到我們使用了一個 url function ，有三個傳入值，第一個傳入值是個 regular expression，在此我們傳入一個空字串，也就是會對應到 url 當中沒有任何東西的時候。而第二個傳入值是個 view function 的位置，這邊我們是傳入剛剛寫的 excuse.views.home

接著切換到你的 terminal，重新輸入

```
python manage.py runserver
```

打開你的瀏覽器，開啓 http://localhost:8000 ，應該就會看到 "It was working in my head" 出現！
