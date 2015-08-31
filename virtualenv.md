## virtualenv

現今大多數 Opensource 專案為了加速開發速度跟減少重複開發的成本，都不免會使用到其他方便的套件或模組，因此最好能夠對於模組的安裝與版本控管建立一套管理的機制。

所幸 Python 可以透過 virtualenv 與 pip 達到簡單的模組安裝與版本控管，提早養成此種好習慣的話，將可有效提高團隊合作開發能力，並且降低團隊成員各自開發環境相依模組版本不同可能帶來的影響，而且 virtualenv 可以有效隔離各自的開發環境，避免因為套件可能產生的衝突。

使用 virtualenv 與 pip 的流程簡述如下：

1. 使用 virtualenv 建立開發環境
2. activate virtualenv 開發環境
3. 使用 pip 安裝 Python 套件
4. 匯出目前安裝的所有套件與版本，並更新專案 requirement 檔案

本文需要安裝的套件如下：

- setuptools
- virtualenv
- pip

接下來本文詳細介紹各個步驟需要使用的相關指令。

###  使用 virtualenv 建立開發環境

利用以下指令，virtualenv 會自動建立一個資料夾，裡面是與系統環境隔絕的 Python 開發環境。

```
$ virtualenv <your_python_project_name>
```

該資料夾內預設會有 3 個子資料夾，分別為 bin/, include/, lib/(如下所示)，其中 bin/ 用於存放 virutalenv 需要用到的指令，include/ 為存放 Python 各版本的標頭檔(.h)，最後 lib/ 則是會被用來存放安裝的套件。

```
your_python_project_name/
        bin/
        include/
        lib/
```

### activate virtualenv 開發環境

啟動 virtualenv 的指令十分簡單，利用 linux 內建的 source 指令即可。

```
$ source <your_python_project_name>/bin/activate
```

啟用成功後，若要停用可使用指令 deactivate 。

```
$ deactivate
```

### 使用 pip 安裝 Python 套件(請記得先啟用 virtualenv )

啟動 virtualenv 後，預設就可以使用 bin/ 資料夾內的 pip 指令進行 Python 套件的安裝，所有透過 pip 安裝的套件，都會自動被安裝到 lib/ 資料夾中，不會被安裝至系統的 Python 執行環境中，因此開發者可以將整個專案資料夾打包之後，交與其他開發者就直接可以執行(但如有需額外編譯的套件，例如 Cython 則是需要再次安裝；此外，若有跨作業系統限制的套件，也需要另外處理)。如此一來，就能夠把整個 Python 專案所需要的套件存放於 virtualenv 所創造的開發環境內，降低了套件相依性可能造成影響，也可保持作業系統內 Python 執行環境的乾淨。

### 匯出目前安裝的所有套件與版本，並更新專案 requirement 檔案

最後，為了能夠確保開發者間所使用的套件版本一致，以避免不同版本造成的相容性與穩定性問題，我們需要將套件版本等資訊一併記錄下來(pip 稱此記錄為 requirement)，未來若有其他開發者想加入時，亦可依照 requirement 內的套件資訊進行安裝。

匯出目前所有已安裝套件與版本的指令如下(請記得先啟用 virtualenv )：

```
pip freeze > requirements.txt
```

安裝 requirements.txt 內的所有套件指令如下(請記得先啟用 virtualenv )：

```
pip install -r requirements.txt
```

## 後記

python 3.3 之後為了做到跟 virtualenv 一樣的事情就多了 `venv` 模組可以使用。

使用方法：

```
$ pyvenv /path/to/new/virtual/environment

# or

$ python -m venv myenv
```

剩下的用法與 virtualenv 相同。
