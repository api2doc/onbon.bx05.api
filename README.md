# onbon bx05 api
本函式庫主要針對 [上海仰邦](http://www.onbonbx.com/) 五代單基色與雙基色顯示屏控制器進行控制與節目的下載，用戶可透過此 API 進行二次開發與系統整合。

若使用 ***BX-5Q*** 全彩控制卡，參考 [BX-5Q 特殊配置](README_5Q.md)。

- [Java 文檔](http://api2doc.github.io/onbon.bx05.api)
- [節目](PROGRAM.md)
- [圖文區](TEXTCAPTION.md)
- [小技巧](TIPS.md)



## 範例
### Client 模式

* 初始化系統環境 (在一個 Process 行程內只需初始化一次)
* 建立屏幕物件
* 連線
* 執行命令
* 斷線

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

* 初始化系統環境 (在一個 Process 行程內只需初始化一次)
* 建立屏幕物件
* 連線
* 執行命令
* 斷線

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

* 初始化系統環境 (在一個 Process 行程內只需初始化一次)
* 建立服務器
* 設定監聽器等待屏幕的連線與斷線事件
* 利用連線發生時的屏幕物件執行命令
* 關閉服務器


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
