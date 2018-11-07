---
title: 大道至簡Hexo
date: 2018-09-04
updated: 2018-09-06
author: yulin
flag: red
tags: 
- hugo
- Github
- leaveit
- 大道
copyright: true
---
# 大道至簡Hexo
Hexo 是一個由[Node.js](http://nodejs.org/)構建的快速、簡單且強大的網誌框架。
# 安裝
## 安裝環境
Mac用戶在編譯時可能會碰到問題，請先至 App Store 安裝 Xcode，一旦 Xcode 安裝完成後，開啟它並前往 **Preferences -> Download -> Command Line Tools -> Install** 安裝命令列工具。
### 安裝Node.js
既然是由Node.js構建的網志框架，那必可不少的環境之一肯定是[Node.js](http://nodejs.org/)。這裏通過安裝包一鍵安裝，亦可通過本地編譯完成安裝。
安裝 Node.js 的最佳方式是透過 [Node Version Manager](https://github.com/creationix/nvm)。感謝 nvm 的開發者提供簡易自動安裝的腳本指令：
cURL:
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
Wget:
```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
一旦安裝完成，重啟終端機並執行下列指令以安裝 Node.js。
```
nvm install stable
```
### 安裝Git
安裝Git的最佳方式是透過[Homebrew](https://mxcl.github.com/homebrew/)安裝。
```
brew install git
```
### 安裝Hexo
在終端下執行下列指令以安裝Hexo。
```
npm install -g hexo-cli
```

截至目前，便完成了Hexo的基本安裝。
# 配置
配置內容，比較複雜，供參考學習的資料網路上也是非常的多，但還是推薦也通讀一遍[Hexo官方文檔](https://hexo.io/zh-tw/docs/)
```
hexo init <folder>
cd <folder>
npm install 
```
如此網誌便已完成建立，在<folder>中會出現以下目錄
- _config.yml /*網站的配置文檔 */
- package.json /*nodejs的安裝包目錄 */
- scaffolds /*當您建立新文章時，Hexo 會根據 scaffold 來建立檔案*/
- source /*原始檔案資料夾，放置檔案內容*/
  - _drafts
  - _posts
 - themes /*主題資料夾*/
# 應用
## 指令
- init
`$ hexo init [folder]`
建立一個新的網站。如果沒有設定 folder 的話，Hexo 會在目前的資料夾建立網站。
- new
`$ hexo new [layout] <title>`
建立一篇新的文章。如果沒有設定 layout 的話，則會使用 _config.yml 中的 default_layout 設定代替。如果標題包含空格的話，請使用引號括起來。
- generate
`$ hexo generate`
產生靜態檔案。
-d, --deploy	產生完成即部署網站
-w, --watch	監看檔案變更
- publish
`$ hexo publish [layout] <filename>`
發表草稿。
- server
`$ hexo server`
啟動伺服器，預設是 http://localhost:4000/。
-p, --port	覆蓋連接埠設定
-s, --static	只使用靜態檔案
-l, --log	啟動記錄器，或覆蓋記錄格式
- deploy
`$ hexo deploy`
部署網站。
-g, --generate	部署網站前先產生靜態檔案
- render
`$ hexo render <file> [file2] ...`
渲染檔案。
-o, --output	輸出位置
- migrate
`$ hexo migrate <type>`
從其他系統 轉移內容。
- clean
`$ hexo clean`
清除快取檔案 (db.json) 和已產生的靜態檔案 (public)。
- list
`$ hexo list <type>`
列出網站資料。
- version
`$ hexo version`
顯示版本資訊。
     - 安全模式`hexo --safe`
             在安全模式下，不會載入外掛和腳本。當您在安裝新外掛後遭遇問題時，可以嘗試以安全模式重新執行。
     - 除錯模式`hexo --debug`
             在終端機中顯示除錯訊息並儲存記錄檔到 debug.log。當您碰到問題時，試著以除錯模式重新執行一次，並 把除錯訊息貼到 GitHub。
    - 安靜模式`hexo --silent`
            隱藏終端機的訊息。

- 自訂配置檔的路徑
`$ hexo --config custom.yml`
自訂配置檔的路徑而不是使用 _config.yml。此參數也接受以逗號分隔的 JSON 或 YAML 檔列表字串 (不得含有空格)，它們將會被合併產生一個 _multiconfig.yml。
`$ hexo --config custom.yml,custom2.json`
顯示草稿
`$ hexo --draft`
顯示 source/_drafts 資料夾中的草稿文章。
自定 CWD
`$ hexo --cwd /path/to/cwd`
自訂目前工作目錄（Current working directory）的路徑。 

立個flag：**9月底完成對hexo的詳細剖析**
