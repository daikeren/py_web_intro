# Django Model & Django Admin

其實到這邊為止我們已經差不多 clone 完 Programmer's Excuses 網站的功能了，只要在 views 裡面多加幾個 excuses 就能夠一直新增更多的藉口！但是我想可能會有疑問：這樣每次都要改程式碼感覺好像有點蠢，不是聽說 Django 是什麼 MVC Framework？又聽說 Django 可以跟資料庫連接，為什麼到現在都還沒提到呢？

別急！這章會教大家怎麼讓 Django 跟資料庫做連接，還有 Django 如何很快地生出一個管理後台來！

## Setting Database

使用 Django 的最大好處之一就是 Django 原生支援許多的資料庫，只要經過簡單的設定，你可以輕鬆從 sqlite 轉換到 MySQL 甚至是 Oracle。為了我們現在開發方便，我們就先用最簡單的 sqlite 吧！

Django 當中跟資料庫相關的設定，都在 settings.py 當中。打開 mysite/settings.py 這個檔案，會看到如下的程式碼：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

在預設設定中，Django 指定了 database engine 為 sqlite3，並將它放在 BASE_DIR 這個目錄（代表你的專案最外層目錄）。你也可以使用其他資料庫，官方支援的除了 SQLite 外尚有 PostgreSQL、MySQL 與 Oracle，另外也有一些非官方的套件可以支援其他資料庫。大部份的資料庫系統中，通常需要指定 database 名稱、使用者、密碼、HOST 以及 PORT，但在這裡我們為了方便說明起見，直接使用預設的 SQLite 設定。

## Django Model

在 Django 當中，我們是透過 Django Model 來跟資料庫做互動。在這邊，我們透過寫 Django Model，讓資料庫能夠產生相對應的 Table，關於 Model 基本的概念如下：

每一個 Django Model 都繼承自 django.db.models.Model
在 Model 當中的每一個 attribute 都代表了一個 database field
Django 讓我們可以透過 Model API 來執行 database query，這代表你可以儘量不用寫 SQL

在這邊，我們要來建立一個存放 Excuses 的 Model，關於 Model 的資訊，我們都會放在 APP 目錄當中的 models.py 裡面。打開 excuse/models.py，輸入

```python
from django.db import models

class Excuse(models.Model):
    content = models.TextField()

    def __str__(self):
        return self.content

```

在此，我們建立了一個叫做 Excuse 的 Model，當中有個叫做 content 的屬性來儲存內容。此外，我們也宣告了一個 __str__(self) function 來表示 Excuse 物件要如何以表示自己，在此我們直接用 Excuse 的內容做表示。

## 實際建立 Database Table

上面的 code 只是告訴 Django table 要長什麼樣子，接下來要真正的在資料庫中把 table 生出來。

要把 table 生出來一樣要使用 manage.py 這個指令，我們透過 syncdb 來產生 table。syncdb 會根據 settings.py 裡面 INSTALLED_APPS 當中的 Django APP 當中的 models.py 中的 Model 生出相對應的 table，打開你的 shell 輸入：

```shell
python manage.py makemigrations
python manage.py migrate
```

就會完成生成 table 的生成。

## 讓我們來玩玩 Model 吧

在 migrate 之後，我們在 terminal 中輸入

```
python manage.py shell
```

來開啟一個跟原生 python 很相像的 interactive shell，跟一般 python 的差別只有我們可以在這個 shell 當中存取 Django Project 當中的 Model 等等資訊。

打開 shell 之後，我們輸入下面的指令

```python
>>> from excuse.models import Excuse
>>> Excuse.objects.create(content="It was working in my head")
>>> Excuse.objects.create(content="I thought I fixed that")
>>> Excuse.objects.create(content="Actually, that is a feature")
```

在這邊，我們創建了 3 個 Excuse 物件，把它們塞到了 database 當中。可以看到我們在這邊沒有寫任何的 SQL 就完成了 insert 資料到 database 的動作。

如果要查詢所有 Excuse 的資料，

```python
>>> Excuse.objects.all()
>>> for excuse in Excuse.objects.all():
>>>     print(excuse.content)
```

其他更多的查詢 Django Model 方法可以參考 [Django Queryset 的官方文件](https://docs.djangoproject.com/en/1.9/ref/models/querysets/)，透過 Django Model 提供的 API，讓你幾乎可以不用寫 SQL 就可以完成許多跟 Database 的操作。

## 修改 Django View

接著我們的 excuse/views.py 要來跟著相對應的修改，改成從 Database 當中拿出來資料。

```python
import random
from excuse.models import Excuse

def home(request):
    excuse = random.choice(Excuse.objects.all())
    return render(request, "index.html", {'excuse': excuse})
```


### 小練習

看看[Django Queryset 的官方文件](https://docs.djangoproject.com/en/1.7/ref/models/querysets/)，還有哪些方法可以用呢？玩玩看吧！

## Django Admin

我們了解到 Django Model 要怎麼建置，也透過了 Django Shell 來對 Model 做操作，但是常常我們會希望能夠直接有個方便的網頁界面可能對這些 Model 做創造、讀取、更新、刪除的動作，那麼該怎麼辦呢？許多的 web framework 可能都需要手動刻一個這樣的界面，但是 Django 當中有 admin 這個套件來幫你很容易地完成相關的事情。

看看在 settings.py 當中的 INSTALLED_APPS 當中已經裝了 'django.contrib.admin' 這個 app

```python
INSTALLED_APPS = (
    'django.contrib.admin',
...
)
```

接著來看看 mysite/urls.py 當中

```python
from django.contrib import admin
admin.autodiscover()

urlpatterns = [
    ...
    url(r'^admin/', include(admin.site.urls)),
]
```

其中 include 這行就是代表把 Django admin 的相關 url 都引入。

接著造訪 http://localhost:8000/admin ，使用你在上一章當中創建的 user/password 登入，如果一切順利的話你會看到 Django Admin 的畫面。不過這個時候 Django Admin 還看不到我們建立的 Excuse 這個 Model，所以我們要告訴 Django Admin 這件事情。

在 excuse 目錄底下，新增 admin.py，內容如下：

```python
from django.contrib import admin

from excuse.models import Excuse

admin.site.register(Excuse)
```

這時候重新 reload http://localhost:8000/admin ，應該就會看到上面出現能夠修改 Excuse 的 admin 界面，我們就可以在這邊做創造、讀取、更新、刪除的動作了。

### 小練習

打開 Django Admin，隨便新增修改 Excuse 幾個試試看吧！
