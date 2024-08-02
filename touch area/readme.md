這段程式碼是一個簡單的畫布應用程式，用來在圖像上繪製多邊形並計算其面積。它支持滑鼠和觸控操作，並且包括校準功能，用於將像素面積轉換為實際的物理面積（平方厘米）。以下是程式碼的各個部分和其作用的解釋：

### 變數和元件
- `canvas` 和 `ctx`: 分別是畫布元素和它的繪圖上下文。
- `imageLoader`, `colorPicker`: 圖片加載器和顏色選擇器的 DOM 元素。
- `redAreaDisplay`, `blueAreaDisplay`, `greenAreaDisplay`: 顯示不同顏色的多邊形面積的 DOM 元素。
- `redMultiplierInput`, `blueMultiplierInput`, `greenMultiplierInput`: 用於輸入每個顏色的倍率。
- `redResultDisplay`, `blueResultDisplay`, `greenResultDisplay`: 顯示計算結果的 DOM 元素。
- `drawing`: 標誌當前是否在繪製多邊形。
- `points`: 當前多邊形的點的數組。
- `img`: 用來加載和顯示圖像的物件。
- `knownBlackArea`: 預設的黑色區域的已知面積（平方厘米）。
- `calibrationRatio`: 用於校準像素面積和實際面積之間的比例。
- `polygons`: 存儲不同顏色的多邊形數據的物件。

### 事件監聽器和處理函數
- `imageLoader.addEventListener('change', handleImage, false);`: 當圖片被加載時調用 `handleImage` 函數。
- `canvas.addEventListener('mousedown', startDrawing);`: 當滑鼠按下時開始繪製多邊形。
- `canvas.addEventListener('mousemove', draw);`: 當滑鼠移動時，若在繪製狀態下，繼續繪製多邊形。
- `canvas.addEventListener('mouseup', endDrawing);`: 當滑鼠釋放時結束繪製。

### 主要函數
- `handleImage(e)`: 處理圖片加載，並進行圖像繪製和校準。
- `startDrawing(event)`: 初始化繪製狀態和起始點。
- `draw(event)`: 繪製過程中記錄點並重繪。
- `endDrawing(event)`: 結束繪製並計算多邊形面積。
- `redraw()`: 清除畫布並重繪所有多邊形。
- `calculateArea(color)`: 計算多邊形的面積，並更新顯示的結果。
- `calculateBlackAreaPixels()`: 計算預定義黑色區域的像素數量並設置校準比例。
- `drawPinkDashedRectangle()`: 在左上角繪製粉紅色虛線矩形，用於顯示校準區域。

### 觸控事件處理
與滑鼠事件類似，但使用觸控事件來支持觸屏設備。
- `canvas.addEventListener('touchstart', startDrawingTouch);`
- `canvas.addEventListener('touchmove', drawTouch);`
- `canvas.addEventListener('touchend', endDrawingTouch);`

### 觸控事件處理函數
- `getRelativeTouchPos(event)`: 計算觸控點相對於畫布的位置。
- `startDrawingTouch(event)`: 觸控開始時初始化繪製狀態和起始點。
- `drawTouch(event)`: 觸控移動時，若在繪製狀態下，記錄點並重繪。
- `endDrawingTouch(event)`: 觸控結束時完成多邊形的繪製和面積計算。

這些函數和事件處理器協同工作，讓使用者能夠在畫布上使用滑鼠或觸控操作來繪製多邊形，並根據圖像的校準比例來計算和顯示這些多邊形的實際面積。
