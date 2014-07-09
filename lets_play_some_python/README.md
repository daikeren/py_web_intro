# Let's Play Some Python

在上一章當中，我們介紹了 Python 的 function 還有 list，接下來我們來看看 Python 還有啥好玩的吧！

## Play with string

在 Python 當中有提供了許多[對字串的操作函式](https://docs.python.org/3/library/string.html)，比如說我們要把字串全部的字母都轉成大寫，就可以使用 [str.upper()](https://docs.python.org/3/library/string.html#string.upper) 這個 function。打開上一章當中我們寫的 views，把最後一行加上 .upper()，看看會有啥結果。

```python
def home(request):
    excuses = [
        'It was working in my head',
        'I thought I fixed that',
        'Actually, that is a feature',
        'It works on my machine',
    ]

    return HttpResponse(excuses[0].upper())
```

### 小練習

看一下關於字串的說明文件，除了 str.upper() 之外還有哪些可以用呢？試試看吧！

## for loop

如果你有任何程式語言的經驗，應該知道迴圈這個概念。在 Python 當中當然也不例外，我們可以用 for loop 來存取一個 list 當中所有的資料。下面是個例子：

```python
def home(request):
    excuses = [
        'It was working in my head',
        'I thought I fixed that',
        'Actually, that is a feature',
        'It works on my machine',
    ]

    output = ''
    for excuse in excuses:
        output += excuse.upper() + '!'

    return HttpResponse(output)
```

在這邊，我們先宣告了一個空字串 output。接著把 excuses 這個 list 當中的每個值取出來，轉成大寫之後再加到 output 的後面。最後再把 output 輸出。重新 reload 瀏覽器，看看會有什麼樣的結果吧！

## Python Standard Library

Python 可愛的不止是它的語法，還有許多的標準函式庫可以使用。詳細的可以在[這邊查看](https://docs.python.org/3/library/)。其實剛剛我們已經用了跟字串相關的 Library 了，接下來我們另外再來介紹一個來玩玩吧！

上一個章節當中，我們無論怎麼 reload 都是輸出 list 當中第一筆的資料。但是為了要讓網站看起來更有趣一點，我們希望可以每次都隨機從 excuses 當中取出一個資料。這樣該怎麼做呢？在這邊，我們可以使用 Python Standard Library 當中的 [random module](https://docs.python.org/3/library/random.html)。一樣是修改 excuse/views.py

```python
import random

def home(request):
    excuses = [
        'It was working in my head',
        'I thought I fixed that',
        'Actually, that is a feature',
        'It works on my machine',
    ]

    return HttpResponse(random.choice(excuses))
```

跟上一個章節的程式碼相比，我們修改了兩個部分。首先，我們多了 `import random` 這一行。在 Python 當中，我們透過 import 來引入某個特定的模組。在這邊，我們告訴我們的程式說要使用 random 這個模組。

接著，我們可以看到最後一樣，我們用了 [random.choice()](https://docs.python.org/3/library/random.html#random.choice) 這個 function。[random.choice()](https://docs.python.org/3/library/random.html#random.choice) 的傳入值是個 list，呼叫之後會回傳 list 當中隨機的一個值。

修改完之後多 reload 幾次你的瀏覽器，看看是不是每次都會出現不同的結果呢？

### 小練習

很快地看一下[Python Standard Library](https://docs.python.org/3/library/)，看看有什麼你覺得有興趣的 module 吧！
