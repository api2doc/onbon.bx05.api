# BX-5Q 系列
針對 BX-5Q 全彩控制卡，在建立屏幕 CLIENT 或 SERVER 物件時，需進行額外配置。

若使用不是 BX-5Q 全彩控制卡，參考 [通用配置](README.md)。

- [Java 文檔](http://api2doc.github.io/onbon.bx05.api)
- [節目](PROGRAM.md)
- [圖文區](TEXTCAPTION.md)
- [小技巧](TIPS.md)

## 範例
### Client 模式

* 初始化系統環境 (在一個 Process 行程內只需初始化一次)
* 建立屏幕物件，並設定此控制器為 onbon.bx05.series.Bx5Q 全彩控制卡。
* 連線
* 執行命令
* 斷線

程式碼如下：
```
Bx5GEnv.initial();

Bx5GScreenClient screen = new Bx5GScreenClient("Hello Screen", new Bx5Q());     // 指定 Bx5Q

screen.connect("192.168.1.1", 5005);

Result<ReturnPingStatus> result1 = screen.ping();
Result<ReturnControllerStatus> result2 = screen.checkControllerStatus();
...

screen.disconnect();
```

### SERVER 模式 (包含 GPRS)

* 初始化系統環境 (在一個 Process 行程內只需初始化一次)
* 建立服務器，並設定此服務器專門處理 onbon.bx05.series.Bx5Q 全彩控制卡。
* 設定監聽器等待屏幕的連線與斷線事件
* 利用連線發生時的屏幕物件執行命令
* 關閉服務器


程式碼如下：

主程式

```
Bx5GEnv.initial();

Bx5GServer server = new Bx5GServer("Hello Screen", new Bx5Q());     // 指定 Bx5Q

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
