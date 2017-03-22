# 節目與區域 (Progarm & Area)

"節目" 主要用於規劃與組合屏幕上顯示的內容，而實際撥放的內容則由 "區域" 控制。

控制器同一時間只能撥放一個節目，須等該節目結束才會撥放下一個。

## 節目

* Java 類為 onbon.bx05.file.ProgramBxFile。

* 節目名稱須符合命命規則 PXXX，XXX 為三位數字，從 001 到 999。

* 節目可配置邊框效果。相關屬性有：

    * 是否播放 FrameShow

    * 效果 FrameStyle

    * 速度 FrameSpeed

    * 步進 FrameMoveStep

    * 載入邊框效果 loadFrameImage

* 節目有效範圍為整個屏幕，實際播放內容由「區域」組成，各區域有自己的位置與大小。

## 區域

* 具備內容被顯示在屏幕上的位置與大小

* 區域不可超過屏幕大小，若節目有邊框，須調整位置與大小避免與邊框效果重疊。

* 區域可配置邊框效果。相關屬性有：

    * 是否播放 FrameShow

    * 效果 FrameStyle

    * 速度 FrameSpeed

    * 步進 FrameMoveStep

    * 載入邊框效果 loadFrameImage

* 區域可顯示的內容有：

    * [圖文區 onbon.bx05.area.TextCaptionBxArea](TEXTCAPTION.md)

    * 時間 onbon.bx05.area.DateTimeBxArea

    * 時鐘 onbon.bx05.area.ClockBxArea

    * 計時 onbon.bx05.area.CounterBxArea

    * 秒表 onbon.bx05.area.TimerBxArea

    * 溫度 onbon.bx05.area.TemperatureBxArea

    * 濕度 onbon.bx05.area.HumidityBxArea

    * 噪音 onbon.bx05.area.NoiseBxArea

## 邊框效果

* 利用 Program 或 Area 的 loadFrameImage 設定效果

* 單色 1-14

* 雙色 1-18

* 全彩 1-17

## 簡易範例

本範例會建立名為 P001 的劇本，並在屏幕 (200, 4) 的位置上，配置一個寬高大小為 160 X 48 的區域顯示圖文內容。

在發送前，會利用 validate 判斷區域是否超出屏幕範圍。

```
// 初始化環境
Bx5GEnv.initial();

// 建立屏幕連線物件
Bx5GScreenClient screen = new Bx5GScreenClient("MyScreen");
if (!screen.connect("192.168.5.7", 5005)) {
    System.out.println("connect failed");
}

// 建立文字內容
TextBxPage page1 = new TextBxPage(new Font("宋体", Font.PLAIN, 12));
page1.newLine("第一篇文字");
page1.newLine("要顯示的內容");
page1.setStayTime(200);

TextBxPage page2 = new TextBxPage(new Font("宋体", Font.PLAIN, 14), Color.green, Color.black);
page2.newLine("第二篇文字");
page2.newLine("其他顯示的內容");
page2.setStayTime(400);

// 建立圖文區域，位置 200, 4，大小 200, 48。
TextCaptionBxArea area = new TextCaptionBxArea(200, 4, 160, 48, screen.getProfile());

// 設定區域邊框效果
area.loadFrameImage(6);

// 將文字內容加入圖文區域內
area.addPage(page1);
area.addPage(page2);

// 建立節目
ProgramBxFile program = new ProgramBxFile("P000", screen.getProfile());

// 設定節目邊框效果
program.loadFrameImage(7);

// 將圖文區域加入節目
program.addArea(area);

// 驗證區域是否超出屏幕範圍
if (program.validate() != null) {
    System.out.println("area out of range");
    return;
}

// 將節目傳送至屏幕顯示
screen.writeProgramQuickly(program);


screen.disconnect();
```
