以下是與滑鼠操作相關的程式碼片段：

1. **滑鼠點擊事件**：
    ```javascript
    // 監聽滑鼠點擊事件
    canvas.addEventListener('click', (event) => {
        const mouseX = event.clientX - canvas.getBoundingClientRect().left;
        const mouseY = event.clientY - canvas.getBoundingClientRect().top;
        // 記錄點擊的座標
        // ...
    });
    ```

2. **多邊形頂點的記錄**：
    ```javascript
    const polygons = {
        red: [],
        blue: [],
        green: [],
    };

    // 在滑鼠點擊事件處理程式中
    function recordVertex(color, x, y) {
        polygons[color].push({ x, y });
    }
    ```

3. **多邊形的繪製**：
    ```javascript
    // 在滑鼠點擊事件處理程式中
    function drawPolygon(color) {
        const poly = polygons[color];
        if (poly.length < 3) return;

        ctx.beginPath();
        ctx.strokeStyle = color;
        ctx.lineWidth = 2;

        ctx.moveTo(poly[0].x, poly[0].y);
        for (let i = 1; i < poly.length; i++) {
            ctx.lineTo(poly[i].x, poly[i].y);
        }
        ctx.closePath();
        ctx.stroke();
    }
    ```

這些程式碼片段可以幫助你實現滑鼠操作和多邊形的繪製功能。如果你有其他問題或需要進一步說明，請隨時告訴我！🙂
