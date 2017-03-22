# 圖文區 (TextCaption Area)

圖文區可加入文字內容、文本檔案、與圖片檔案。並根據配置的效果呈現在屏幕上。

* 文字內容 onbon.bx05.area.page.TextBxPage

* 文本檔案 onbon.bx05.area.page.TextFileBxPage

* 圖片檔案 onbon.bx05.area.page.ImageFileBxPage

## 效果

常用的播放效果有：

* 1：静止显示
* 2：快速打出
* 3：向左移动
* 4：向左连移
* 5：向上移动
* 6：向上连移

完整清單參考 [javadoc](http://api2doc.github.io/onbon.bx05.api) 的 [ onbon.bx05.utils.DisplayStyleFactory](http://api2doc.github.io/onbon.bx05.api/onbon/bx05/utils/DisplayStyleFactory.html) 說明，

## 簡易範例：文字內容

本範例會在屏幕 (100, 20) 的位置上，配置一個寬高大小為 400 X 100 的區域顯示圖文內容。

* 字型選用 Tahoma 16 號
* 文字顏色為綠色
* 向左連移
* 連移速度為 15

```
// 建立圖文區域
TextCaptionBxArea area = new TextCaptionBxArea(10, 20, 400, 100, screen.getProfile());
area.setFrameShow(false);

// 建立文字內容
TextBxPage page1 = new TextBxPage(new Font("宋体", Font.PLAIN, 14));
page.setLineBreak(false);   // 換行不換頁，可讓多行的文字顯示在同一頁上。
page.newLine("第一篇");
page.newLine("這是仰邦 Java API");
page.setStayTime(200);  // 翻頁時間，10 ms 為單位。
page.setDisplayStyle(DisplayStyleFactory.getStyle(2));  // 快速打出。

// 將文字內容加入圖文區域
area.addPage(page);
```

## 簡易範例：文本

本範例會在屏幕 (50, 30) 的位置上，配置一個寬高大小為 350 X 200 的區域顯示文本為 C:\TEMP\NEWS.TXT 的內容。

* 字型選用 Tahoma 14 號
* 文字顏色為紅色
* 向上連移
* 連移速度為 10

```
// 建立圖文區域
TextCaptionBxArea area = new TextCaptionBxArea(50, 30, 350, 200, screen.getProfile());
area.setFrameShow(false);

// 建立文字內容
TextFileBxPage page = new TextFileBxPage("c:\\temp\\news.txt");
page.setFont(new Font("Tahoma", Font,Plain, 14));
page.setForeground(Color.RED);
page.setDisplayStyle(DisplayStyleFactory.getStyle(6));
page.setSpeed(10);

// 將文字內容加入圖文區域
area.addPage(page);
```
