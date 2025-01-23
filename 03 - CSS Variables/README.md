### **JavaScript30 - Day 3: Playing with CSS Variables and JS**

Day 3 的主題是 **用 JavaScript 操控 CSS 變數**，實現動態改變樣式的效果（例如：調整圖片的模糊度、邊框間距和顏色等）。

---

## **大目標**
通過 JavaScript 操控 **CSS 變數（Custom Properties）**，實現以下功能：
1. 動態調整圖片邊框的間距。
2. 動態調整圖片的模糊程度。
3. 動態改變圖片的邊框顏色。

---

## **學習重點**
1. **CSS 變數（Custom Properties）：**
   - CSS 中的變數定義和使用方式。
2. **使用 JavaScript 操控 CSS 變數：**
   - 如何讀取和更改 CSS 變數的值。
3. **監聽滑桿（`<input type="range">`）的事件：**
   - 使用 `input` 事件動態更新 CSS 樣式。

---

## **拆解步驟**

### **1. 建立基本的 HTML 結構**

這裡需要：
1. 一張圖片。
2. 幾個滑桿（Range Slider）用於調整圖片的樣式（間距、模糊度、邊框顏色）。

#### **HTML 範例**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Day 3: CSS Variables</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Update CSS Variables with <span class="hl">JS</span></h1>
  <div class="controls">
    <!-- 滑桿調整間距 -->
    <label for="spacing">Spacing:</label>
    <input id="spacing" type="range" name="spacing" min="10" max="200" value="10" data-sizing="px">
    
    <!-- 滑桿調整模糊度 -->
    <label for="blur">Blur:</label>
    <input id="blur" type="range" name="blur" min="0" max="25" value="10" data-sizing="px">
    
    <!-- 調整邊框顏色 -->
    <label for="base">Base Color:</label>
    <input id="base" type="color" name="base" value="#ff6600">
  </div>
  
  <!-- 圖片 -->
  <img src="https://source.unsplash.com/7bwQXzbF6KE/800x500" alt="Example">
  
  <script src="script.js"></script>
</body>
</html>
```

---

### **2. 設置 CSS 樣式**

在 CSS 中定義變數和樣式，這些變數將由 JavaScript 控制更新。

#### **CSS 範例**
```css

/*CSS 變數（Custom Properties）:
  CSS 變數是一種可以重複使用的值，通常用來定義顏色、尺寸、間距等樣式參數。所有 CSS 變數的命名都必須以 -- 開頭。並且只能在 :root 或其他選擇器內定義。
    - 定義 CSS 變數：--變數名稱: 值;
    - 使用 CSS 變數：屬性: var(--變數名稱);

為什麼不能直接用 --box-color？
  CSS 的規範要求：
    - 變數的定義（例如 --box-color）只是存儲數值。
    - 使用變數時，必須用 var() 函數調用變數值。
    - 如果直接寫 --box-color，CSS 會認為這是無效的語法，因為只有 var(--變數名) 才是合法的。
*/
:root {
  --base: #ff6600; /* 初始顏色 */
  --spacing: 10px; /* 初始邊距 */
  --blur: 10px;    /* 初始模糊度 */
}

body {
  font-family: sans-serif;
  text-align: center;
  background: #f4f4f4;
  color: #333;
}

img {
  max-width: 100%;
  border: 10px solid var(--base);
  padding: var(--spacing);
  filter: blur(var(--blur));
  transition: all 0.3s ease;
}

.controls {
  margin-bottom: 20px;
}

label {
  margin-right: 10px;
}

input[type="range"] {
  margin: 0 10px;
}
```

#### **CSS 設計重點**
1. **CSS 變數定義：**
   - 使用 `:root` 定義全局變數（`--base`、`--spacing`、`--blur`）。
   - 例如：
     ```css
     --spacing: 10px;
     ```

2. **使用 CSS 變數：**
   - 在樣式中引用變數，格式是 `var(--變數名稱)`。
   - 例如：
     ```css
     padding: var(--spacing);
     border: 10px solid var(--base);
     filter: blur(var(--blur));
     ```

---

### **3. 使用 JavaScript 操控 CSS 變數**

JavaScript 主要負責：
1. **監聽滑桿的變化（`input` 事件）**。
2. **更新 CSS 變數的值**。
<!-- JavaScript 操控 CSS 變數的流程是：

1.選取目標元素：
  - 使用 document.documentElement 或特定 DOM 元素。
2.讀取或修改變數：
  - 透過 style.getPropertyValue() 讀取 CSS 變數的值。
  - 透過 style.setProperty() 修改 CSS 變數的值 -->

#### **核心邏輯**
```javascript
// 取得所有滑桿和顏色選擇器
const inputs = document.querySelectorAll('.controls input');

function handleUpdate() {
  // 取得 data-sizing 屬性（如 px）或空字串
  const suffix = this.dataset.sizing || '';
  
  // 更新 CSS 變數
  document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
}

// 監聽每個滑桿的 input 事件
inputs.forEach(input => input.addEventListener('input', handleUpdate));
```

---

### **4. 整體解釋**

#### **步驟 1：選取滑桿元素**
用 `querySelectorAll` 選取所有的 `<input>` 元素。

#### **步驟 2：監聽 `input` 事件**
滑桿在每次滑動時會觸發 `input` 事件，我們通過 `addEventListener` 監聽該事件。

#### **步驟 3：動態更新 CSS 變數**
1. 每個 `<input>` 元素都有 `name` 和 `value` 屬性。
   - 例如：
     - `name="spacing"`，`value="20"`。
2. 利用 `style.setProperty()` 更新 CSS 變數。
   - 格式：
     ```javascript
     document.documentElement.style.setProperty(`--變數名稱`, 值);
     ```

---

### **完整 JavaScript 範例**
```javascript
const inputs = document.querySelectorAll('.controls input');

function handleUpdate() {
  const suffix = this.dataset.sizing || ''; // 取得單位，例如 px
  document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
}

inputs.forEach(input => input.addEventListener('input', handleUpdate));
```

---