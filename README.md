# bgmi-dockerlized

bgmi-dockerlized是BGmi的第三方Docker化鏡像

## 前言

找不到好用的docker鏡像，[BGmi-docker-all-in-one](https://github.com/BGmi/bgmi-docker-all-in-one)又不能配合compose用，就重新做一個純BGmi的


## 安裝
### 自行編譯
```
git clone --recurse-submodules https://git.tkmhome.com/fukhak/bgmi-dockerlized.git
docker build -t fukhak/bgmi ./bgmi-dockerlized
```

### 或者用Docker Compose運行
瀏覽[fukhak\bgmi-compose](https://git.tkmhome.com/fukhak/bgmi-compose)作更進一步瞭解

## 運行
鏡像默認使用/app以及/data作為保存設定以及貯存影片之用途，亦有使用volume。假如忘記掛載可以自己在docker volume中找回

### 運行BGmi服務:
```
docker run -it -v $(pwd)/app:/app -v $(pwd)/data:/data -p 80:80 fukhak/bgmi
```

暫時加了個crontask，每半小時會用bgmi update --download更新一次

環境參數:
不建議覆蓋BGMI_PATH和SAVE_PATH參數，可能影響程序處理。儘量使用-v 本機路徑:/app以及-v 本機路徑:/data

其他參數參考BGmi使用請參考[官方手冊](https://github.com/BGmi/BGmi/blob/master/README.cn.md#使用)各項設置的意義

可以使用的環境參數

只會在沒有配置文件時運行！！！
|KEY|默認/可選參數|備注|
|---|---|---|
|BGMI_PATH|"/app"|配置文件夾路徑|
|BANGUMI_PATH|"/data"|影片路徑|
|DATA_SOURCE|"bangumi_moe", "mikan_project", "dmhy"||
|BANGUMI_MOE_URL|"https://bangumi.moe"||
|SHARE_DMHY_URL|"https://share.dmhy.org"||
|DOWNLOAD_DELEGATE|"aria2-rpc", "transmission-rpc", "deluge-rpc"||
|TMP_PATH|"/app/tmp"||
|MAX_PAGE|3||
|DANMAKU_API_URL|""||
|LANG|"zh_cn", "zh_tw", "ja", "en"||
|ADMIN_TOKEN|[隨機生成]||
|ENABLE_GLOBAL_FILTER|1|想令GLOBAL_FILTER生效必須為1|
|GLOBAL_FILTER|"Leopard-Raws, hevc, x265, c-a Raws, U3-Web"|使用“DEFAULT"或者"default"會變成左邊的參數|
|TORNADO_SERVE_STATIC_FILES|0|假如使用內置http作為前端請使用1|

---

#### DOWNLOAD_DELEGATE="aria2-rpc"需填寫:

|KEY|
|---|
|ARIA2_RPC_URL||
|ARIA2_RPC_TOKEN|
---

#### DOWNLOAD_DELEGATE="transmission-rpc"需填寫:

|KEY|
|---|
|TRANSMISSION_RPC_URL
|TRANSMISSION_RPC_PORT
|TRANSMISSION_RPC_USERNAME
|TRANSMISSION_RPC_PASSWORD
---

#### DOWNLOAD_DELEGATE=""需填寫:

|KEY|
|---|
|DELUGE_RPC_URL|
|DELUGE_RPC_PASSWORD|
---

### 以指令形式運行單獨運行
```
docker run -it fukhak/bgmi [command] [arg1] [arg2] ...
```

例子:

修改站源
```
docker run -it -v $(pwd)/app:/app -v $(pwd)/data:/data fukhak/bgmi bgmi source bangumi_moe
```

運行BGmi-http
```
docker run -it -v $(pwd)/app:/app -v $(pwd)/data:/data fukhak/bgmi bgmi-http
```

### 在container中使用
```
docker container ls
docker exec containerid bash
```

---

## 刪除
```
docker rmi fukhak/bgmi
```
