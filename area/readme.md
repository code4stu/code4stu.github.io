1. **area.html**：
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Polygon Area Calculator</title>
       <link rel="stylesheet" href="polygon-area-calculator.css">
   </head>
   <body>
       <h1>Polygon Area Calculator</h1>
       <input type="file" id="imageLoader" name="imageLoader"/>
       <div class="canvas-container">
           <canvas id="canvas" width="640" height="480"></canvas>
           <div class="controls">
               <label for="colorPicker">選擇顏色:</label>
               <select id="colorPicker">
                   <option value="red">紅色</option>
                   <option value="blue">藍色</option>
                   <option value="green">綠色</option>
               </select>
               <div class="results">
                   <p>紅色區域面積: <span id="redArea">0</span> cm²</p>
                   <input type="number" id="redMultiplier" value="80" onchange="calculateArea('red')">
                   <span>結果: <span id="redResult">0</span> 根</span>
                   <p>藍色區域面積: <span id="blueArea">0</span> cm²</p>
                   <input type="number" id="blueMultiplier" value="40" onchange="calculateArea('blue')">
                   <span>結果: <span id="blueResult">0</span> 根</span>
                   <p>綠色區域面積: <span id="greenArea">0</span> cm²</p>
                   <input type="number" id="greenMultiplier" value="20" onchange="calculateArea('green')">
                   <span>結果: <span id="greenResult">0</span> 根</span>
               </div>
           </div>
       </div>
       <script src="script.js"></script>
   </body>
   </html>
   ```
   - **描述**：這個 HTML 檔案是多邊形面積計算器的主要界面。用戶可以通過上傳圖片、選擇顏色、調整倍數來計算不同顏色多邊形的面積。
   - **元素**：
     - `imageLoader`：用於上傳圖片的 `<input>` 元素。
     - `canvas`：用於顯示圖片和多邊形的 `<canvas>` 元素。
     - `colorPicker`：選擇顏色的 `<select>` 元素。
     - `redArea`, `blueArea`, `greenArea`：顯示各顏色區域面積的 `<span>` 元素。
     - `redMultiplier`, `blueMultiplier`, `greenMultiplier`：調整倍數並觸發計算面積的 `<input>` 元素。
     - `redResult`, `blueResult`, `greenResult`：顯示計算結果的 `<span>` 元素。

2. **polygon-area-calculator.css**：
   ```css
   body {
       display: flex;
       flex-direction: column;
       align-items: center;
       font-family: Arial, sans-serif;
   }

   .canvas-container {
       display: flex;
       align-items: flex-start;
   }

   canvas {
       border: 1px solid black;
   }

   .controls {
       margin-left: 20px; /* 加入左側間距，使控制項目與畫布保持距離 */
       display: flex;
       flex-direction: column;
       align-items: flex-start;
   }

   .controls > * {
       margin-top: 10px; /* 調整每個控制項目之間的上間距 */
       margin-bottom: 10px; /* 調整每個控制項目之間的下間距 */
   }
   ```
   - **描述**：這個 CSS 檔案定義了多邊形面積計算器的外觀和排版樣式。
   - **風格**：
     - `body`：使整個應用程式居中顯示並使用指定字體。
     - `.canvas-container`：控制畫布和控制項之間的排版。
     - `canvas`：設置畫布邊框樣式。
     - `.controls`：調整控制項的排列和間距。

3. **script.js**：
   ```javascript
    function calculateArea(color) {
    const poly = polygons[color][polygons[color].length - 1];
    if (poly.length < 3 || calibrationRatio === null) return;

    let area = 0;
    for (let i = 0; i < poly.length; i++) {
        const j = (i + 1) % poly.length;
        area += poly[i].x * poly[j].y;
        area -= poly[j].x * poly[i].y;
    }
    area = Math.abs(area) / 2;

    const areaCm2 = area * calibrationRatio;
    console.log(`Calculated area in pixels: ${area}, Calibration ratio: ${calibrationRatio}, Area in cm²: ${areaCm2}`);
    
    // 更新顯示的面積
    if (color === 'red') {
        redAreaDisplay.textContent = areaCm2.toFixed(2);
        redResultDisplay.textContent = (areaCm2 * parseFloat(redMultiplierInput.value)).toFixed(2);
    } else if (color === 'blue') {
        blueAreaDisplay.textContent = areaCm2.toFixed(2);
        blueResultDisplay.textContent = (areaCm2 * parseFloat(blueMultiplierInput.value)).toFixed(2);
    } else if (color === 'green') {
        greenAreaDisplay.textContent = areaCm2.toFixed(2);
        greenResultDisplay.textContent = (areaCm2 * parseFloat(greenMultiplierInput.value)).toFixed(2);
    }
}

function calculateBlackAreaPixels() {
    // 取得左上角從 (0, 0) 開始，寬高為 100 像素
    const blackRegion = ctx.getImageData(0, 0, 100, 100); 
    let blackPixels = 0;

    // 遍歷這個區域的每個像素
    for (let i = 0; i < blackRegion.data.length; i += 4) {
        // 每個像素有 4 個值 (R, G, B, A)
        const [r, g, b, a] = blackRegion.data.slice(i, i + 4);
        // 判斷這個像素是否是黑色 (閾值 < 50，透明度 > 200)
        if (r < 50 && g < 50 && b < 50 && a > 200) {
            blackPixels++;
        }
    }

    // 計算校準比例
    if (blackPixels > 0) {
        calibrationRatio = knownBlackArea / blackPixels;
    } else {
        calibrationRatio = null;
    }
    console.log(`黑色區域的像素數: ${blackPixels}, 校準比例: ${calibrationRatio}`);
}

function drawPinkDashedRectangle() {
    // 繪製粉紅色虛線矩形
    ctx.strokeStyle = 'pink'; 
    ctx.lineWidth = 2;
    ctx.setLineDash([5, 5]); 
    ctx.strokeRect(0, 0, 100, 100); // 粉紅色虛線矩形的位置和大小
}
```

- **功能**：
  - `calculateArea(color)` 函數計算指定顏色多邊形區域的面積，並更新相應的 HTML 元素顯示。
  - `calculateBlackAreaPixels()` 函數計算畫布左上角 100x100 像素區域內的黑色像素數，並根據已知的黑色區域面積計算校準比例。
  - `drawPinkDashedRectangle()` 函數在畫布上繪製一個粉紅色虛線矩形，用於標記校準區域。

這三個檔案一起工作，提供了一個完整的多邊形面積計算器，讓用戶能夠上傳圖片、標記多邊形、計算面積並顯示結果。
