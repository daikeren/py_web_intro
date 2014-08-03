# 安裝 Django

在開始之前，我們先在我們的根目錄下面建立一個我們工作的目錄。

```
mkdir pyweb_intro
```

切換到我們剛剛建立的目錄下

```
cd pyweb_intro
```

## 安裝與設定 Python

既然 Django 是用 Python 寫成，我們首先需要安裝 Python。如果你使用 Linux 或 OS X，Python 已經內建於系統中，可以跳過此節。

Windows 使用者可以先[前往官網下載 Python](https://www.python.org/download/)。本教學基於 Python 2.7，所以請下載 **Python 2.7 Windows Installer**。最後面的版本號可能會不同（例如 2.7.6），但只要挑 2.7 開頭的下載即可。

下載後雙擊執行，下一步到底就安裝完成了。預設的安裝位置會在 `C:\Python27`。試著[打開命令提示字元視窗](http://windows.microsoft.com/zh-tw/windows/command-prompt-faq)，輸入以下指令

```
C:\Python27\python
```

如果出現類似以下的內容：

```
Python 2.7.6 (default, Apr  9 2014, 11:48:52) [MSC v.1600 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

就代表安裝成功了。可以輸入 `exit()` 退出。

但是每次都要輸入這麼長一串才能執行 Python 有點麻煩。藉由設定 `PATH` 環境變數，可以讓我們省略前面的路徑。打開控制台→系統，點選「進階系統設定」。在彈開的視窗中點選「環境變數」，會出現一個標題為「環境變數」的視窗。在這個視窗的上半部找到「Path」這個變數，雙擊後面的「值」欄位，在「變數值」欄位的最前面加上 `C:\Python27;C:\Python27\Scripts\`，接著一路確定關閉所有設定視窗。

重新打開命令提示字元，試著輸入 `python`。你應該會看到和剛剛類似的內容，代表你已經成功讓系統認識到 Python 的安裝位置。

## virtualenv

在我們開發 Python 專案的時候，我們通常都會使用 virtualenv。使用 virtualenv 的好處如下：

1. 我們會希望可以為每一個專案創造出獨立的環境。如此才不會因為其他專案的 Library 升級而導致專案出問題。
2. 當我們的專案要移到其他機器上頭的時候，相依的套件比較好處理


在 Ubuntu 當中可以直接用 apt-get 來安裝 virtualenv。

```
sudo apt-get install python-virtualenv
```

在其他的系統上，我們建議使用 pip 安裝 virtualenv。

### 安裝 Virtualenv（非 Ubuntu）

首先我們需要確認你的系統上有沒有安裝 pip。直接輸入以下指令：

```
pip
```

如果看到以下內容：

```
Usage:
  pip <command> [options]

Commands:
  install                     Install packages.
  uninstall                   Uninstall packages.
[以下略…]
```

就代表你已經有 pip 了，可以跳過下一個小節，直接開始安裝 virtualenv。

#### 安裝 Pip

下載 [`get-pip.py`](https://bootstrap.pypa.io/get-pip.py) 這個檔案。切換至該檔案的目錄，用 Python 執行它：

```
python get-pip.py
```

即可完成安裝。完成後請再次試著執行 `pip` 指令，以確認安裝是否成功。

## Install Django

安裝 Django 十分簡單，只要透過 pip 就可以完成。在 terminal 底下輸入：

```
pip install django
```

便會下載最新版的 django 並且完成安裝。
