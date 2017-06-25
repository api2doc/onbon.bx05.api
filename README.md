 onbon bx05 api
=====================

最新版本 [Version 0.5.0-UDPATE.3](https://github.com/api2doc/onbon.bx05.api/releases/tag/v0.5.0-UPDATE.3)

本函式庫主要針對 [上海仰邦](http://www.onbonbx.com/) 五代單基色與雙基色顯示屏控制器進行控制與節目的下載，用戶可透過此 API 進行二次開發與系統整合。

若使用 ***BX-5Q*** 全彩控制卡，參考 [BX-5Q 特殊配置](README_5Q.md)。

- [Java 文檔](http://api2doc.github.io/onbon.bx05.api)

- [節目](PROGRAM.md)

- [圖文區](TEXTCAPTION.md)

- [小技巧](TIPS.md)

- [安卓設定](https://github.com/api2doc/onbon.bx05.mobiledemo) (僅 onbon bx05 api 0.5.0 或以上支援)

## 執行環境
- Java 6 或以上

- Andorid 5.0 或以上 (僅 onbon bx05 api 0.5.0 或以上支援)

## Android 開發時需要參考的 JAR 檔 (7/10)
- bx05.message-0.5.0-SNAPSHOT.jar

- bx05-0.5.0-SNAPSHOT.jar

- log4j-1.2.14.jar

- simple-xml-2.7.1.jar

- uia.comm-0.2.1.jar

- uia.message-__x.x.x__.jar (__x.x.x__ 由各版本決定)

- uia.utils-__x.x.x__.jar (__x.x.x__ 由各版本決定)


## 範例
### Client 模式
- 初始化系統環境 (在一個 Process 行程內只需初始化一次)
- 建立屏幕物件
- 連線
- 執行命令
- 斷線

程式碼如下：
```
Bx5GEnv.initial();

Bx5GScreenClient screen = new Bx5GScreenClient("Hello Screen");

screen.connect("192.168.1.1", 5005);

Result<ReturnPingStatus> result1 = screen.ping();
Result<ReturnControllerStatus> result2 = screen.checkControllerStatus();
...

screen.disconnect();
```

### RS232 模式
- 初始化系統環境 (在一個 Process 行程內只需初始化一次)
- 建立屏幕物件
- 連線
- 執行命令
- 斷線

程式碼如下：
```
Bx5GEnv.initial();

Bx5GScreenRS screen = new Bx5GScreenRS("Hello Screen");

screen.connect("COM3", BaudRate.RATE_57600);

Result<ReturnPingStatus> result1 = screen.ping();
Result<ReturnControllerStatus> result2 = screen.checkControllerStatus();
...

screen.disconnect();
```

### SERVER 模式 (包含 GPRS)
- 初始化系統環境 (在一個 Process 行程內只需初始化一次)
- 建立服務器
- 設定監聽器等待屏幕的連線與斷線事件
- 利用連線發生時的屏幕物件執行命令
- 關閉服務器

程式碼如下：

主程式

```
Bx5GEnv.initial();

Bx5GServer server = new Bx5GServer("Hello Screen", 8036);

server.addListener(new ConnectionListener());

server.start();

System.out.println("waiting 120 secs");
Thread.sleep(120000);

server.stop();
System.out.println("done!");

```

監聽程式

```
public class ConnectionListener implements Bx5GServerListener {

    @Override
    public void connected(String socketId, String controllerAddr, Bx5GScreen screen) {
        Result<ReturnPingStatus> result1 = screen.ping();
        Result<ReturnControllerStatus> result2 = screen.checkControllerStatus();
        ...
    }

    @Override
    public void disconnected(String socketId, String controllerAddr, Bx5GScreen screen) {
        ...
    }
}
```
