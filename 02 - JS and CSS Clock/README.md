### **Day02: CSS + JS Clock**

---

## **大目標**
製作一個動態時鐘，結合 CSS 和 JavaScript，讓時針、分針、秒針根據當前時間轉動。

---

### **學習重點**
1. **DOM 操作**：如何選取元素並更新樣式。
2. **CSS `transform`**：用來旋轉指針。
3. **時間操作**：如何用 JavaScript 的 `Date` 物件處理時間。
4. **動態更新**：使用 `setInterval` 定期更新時鐘狀態。

---

### **拆解小目標**
1. **HTML**：建立時鐘表盤和指針結構。
2. **CSS**：設定時鐘的樣式，包括指針的定位和動畫效果。
3. **JavaScript**：
   - 獲取當前時間。
   - 計算指針的旋轉角度。
   - 更新指針位置並讓它每秒動態刷新。

---

## **詳細內容**

### **1. 準備 HTML 結構**
HTML 的結構包含一個 `clock` 容器，內有 `hour-hand`、`minute-hand` 和 `second-hand` 三個指針。

#### **HTML**
```html
<div class="clock">
  <div class="clock-face">
    <div class="hand hour-hand"></div>
    <div class="hand minute-hand"></div>
    <div class="hand second-hand"></div>
  </div>
</div>
```

---

### **2. 設置 CSS 樣式**
1. 設定時鐘的基本樣式，包括圓形表盤。
2. 設定指針的樣式，並使用 `transform-origin` 來定義旋轉的起點。

#### **CSS**
```css
body {
  background: #282c34;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  font-family: sans-serif;
}

.clock {
  width: 300px;
  height: 300px;
  border: 10px solid white;
  border-radius: 50%;
  position: relative;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.clock-face {
  position: relative;
  width: 100%;
  height: 100%;
  transform: translateY(-3px);
}

.hand {
  width: 50%;
  height: 6px;
  background: white;
  position: absolute;
  top: 50%;
  transform-origin: 100%;
  transform: rotate(90deg); /* 初始化為垂直方向 */
  transition: all 0.05s ease;
}
```

---

### **3. JavaScript 詳細步驟拆解**

---

#### **a. 選取指針元素**
用 `document.querySelector` 選取三個指針，分別是秒針、分針和時針。
```javascript
const secondHand = document.querySelector('.second-hand');
const minuteHand = document.querySelector('.minute-hand');
const hourHand = document.querySelector('.hour-hand');
```

---

#### **b. 獲取當前時間**
使用 `Date` 物件取得當前時間的秒、分和小時，計算指針的旋轉角度。

### **使用的語法：`new Date()`**
- **語法格式：**
  ```javascript
  const now = new Date();
  ```
- **功能：**
  - `Date` 是 JavaScript 的內建物件，用來處理日期和時間。
  - `new Date()` 會建立一個代表當前時間的日期物件。
- **範例：**
  ```javascript
  const now = new Date();
  console.log(now); // 例如：Sat Jan 16 2025 15:30:45 GMT+0800
  ```

### **使用的語法：`getSeconds`、`getMinutes`、`getHours`**
- **語法格式：**
  ```javascript
  dateObject.getSeconds()
  dateObject.getMinutes()
  dateObject.getHours()
  ```
- **功能：**
  - `getSeconds()`：取得當前的秒數（0～59）。
  - `getMinutes()`：取得當前的分鐘數（0～59）。
  - `getHours()`：取得當前的小時數（0～23）。

```javascript
const now = new Date(); // 獲取目前時間
const seconds = now.getSeconds(); // 獲取秒數（0~59）
const minutes = now.getMinutes(); // 獲取分鐘數（0~59）
const hours = now.getHours(); // 獲取小時數（0~23）
```

---

#### **c. 計算指針的旋轉角度**
1. 秒針的旋轉角度：每秒旋轉 6 度（360 ÷ 60）。
   ```javascript
   const secondsDegrees = ((seconds / 60) * 360) + 90; // +90 是為了校正初始位置
   ```
2. 分針的旋轉角度：每分鐘旋轉 6 度。
   ```javascript
   const minutesDegrees = ((minutes / 60) * 360) + 90;
   ```
3. 時針的旋轉角度：每小時旋轉 30 度（360 ÷ 12）。
   ```javascript
   const hoursDegrees = ((hours / 12) * 360) + 90;
   ```

---

#### **d. 更新指針位置**
用 `style.transform` 來改變指針的旋轉角度。
```javascript
secondHand.style.transform = `rotate(${secondsDegrees}deg)`;
minuteHand.style.transform = `rotate(${minutesDegrees}deg)`;
hourHand.style.transform = `rotate(${hoursDegrees}deg)`;
```

---

#### **e. 每秒更新一次**
使用 `setInterval` 讓時鐘每秒自動更新。
```javascript
function setClock() {
  const now = new Date();

  const seconds = now.getSeconds();
  const secondsDegrees = ((seconds / 60) * 360) + 90;
  secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

  const minutes = now.getMinutes();
  const minutesDegrees = ((minutes / 60) * 360) + 90;
  minuteHand.style.transform = `rotate(${minutesDegrees}deg)`;

  const hours = now.getHours();
  const hoursDegrees = ((hours / 12) * 360) + 90;
  hourHand.style.transform = `rotate(${hoursDegrees}deg)`;
}

setInterval(setClock, 1000); // 每秒執行一次
setClock(); // 初始化時執行一次
```

---


## **步驟 2：取得當前時間**
我們需要取得當前的「秒」、「分」和「小時」來計算指針的旋轉角度。

#### **在這個項目中的應用**
```javascript
const now = new Date(); // 取得當前時間
const seconds = now.getSeconds(); // 取得當前的秒數
const minutes = now.getMinutes(); // 取得當前的分鐘數
const hours = now.getHours(); // 取得當前的小時數
```

---

## **步驟 3：計算旋轉角度**
我們需要根據時間計算秒針、分針和時針的旋轉角度。

### **使用的語法：數學計算**
1. **除法（`/`）：**
   - 用來計算一個數字被另一個數字除的結果。
   - 例如：`seconds / 60` 代表「當前秒數占總秒數的比例」。

2. **乘法（`*`）：**
   - 用來計算一個數字乘以另一個數字。
   - 例如：`(seconds / 60) * 360` 代表「當前秒數對應的角度」。

3. **加法（`+`）：**
   - 用來將兩個數字相加。
   - 例如：`((seconds / 60) * 360) + 90` 代表「加上初始校正角度 90 度」。

#### **在這個項目中的應用**
1. **秒針的角度：**
   ```javascript
   const secondsDegrees = ((seconds / 60) * 360) + 90;
   ```
   - `seconds / 60`：計算秒針走過的比例。
   - `* 360`：將比例轉換為角度（時鐘是 360 度）。
   - `+ 90`：校正初始位置。

2. **分針的角度：**
   ```javascript
   const minutesDegrees = ((minutes / 60) * 360) + 90;
   ```

3. **時針的角度：**
   ```javascript
   const hoursDegrees = ((hours / 12) * 360) + 90;
   ```

---

## **步驟 4：更新指針位置**
我們需要將計算出的角度應用到指針的旋轉上。

### **使用的語法：`style.transform`**
- **語法格式：**
  ```javascript
  element.style.transform = 'rotate(angle)';
  ```
- **功能：**
  - 改變元素的 CSS `transform` 屬性，這裡用來控制指針的旋轉。
  - `rotate(angle)` 是旋轉的函式，`angle` 是角度（以度數為單位，`deg`）。

- **範例：**
  假設我們要將一個指針旋轉到 180 度：
  ```javascript
  secondHand.style.transform = 'rotate(180deg)';
  ```

#### **在這個項目中的應用**
1. 更新秒針：
   ```javascript
   secondHand.style.transform = `rotate(${secondsDegrees}deg)`;
   ```
2. 更新分針：
   ```javascript
   minuteHand.style.transform = `rotate(${minutesDegrees}deg)`;
   ```
3. 更新時針：
   ```javascript 
   hourHand.style.transform = `rotate(${hoursDegrees}deg)`;
   ```

---

## **步驟 5：讓時鐘動起來**
我們需要讓時鐘每秒自動更新。

### **使用的語法：`setInterval`**
- **語法格式：**
  ```javascript
  setInterval(function, milliseconds)
  ```
- **功能：**
  - 每隔一定的時間（以毫秒為單位）重複執行某個函數。
  - `function` 是要執行的函數，`milliseconds` 是執行的間隔時間。

- **範例：**
  每秒執行一次 `updateClock` 函數：
  ```javascript
  setInterval(updateClock, 1000);
  ```

#### **在這個項目中的應用**
```javascript
setInterval(setClock, 1000); // 每秒執行一次 setClock 函數
```

---

## **完整程式碼**
```javascript
// 選取指針
const secondHand = document.querySelector('.second-hand');
const minuteHand = document.querySelector('.minute-hand');
const hourHand = document.querySelector('.hour-hand');

// 定義函數：計算指針位置並更新樣式
function setClock() {
  const now = new Date();

  // 計算秒針角度
  const seconds = now.getSeconds();
  const secondsDegrees = ((seconds / 60) * 360) + 90;
  secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

  // 計算分針角度
  const minutes = now.getMinutes();
  const minutesDegrees = ((minutes / 60) * 360) + 90;
  minuteHand.style.transform = `rotate(${minutesDegrees}deg)`;

  // 計算時針角度
  const hours = now.getHours();
  const hoursDegrees = ((hours / 12) * 360) + 90;
  hourHand.style.transform = `rotate(${hoursDegrees}deg)`;
}

// 每秒更新一次時鐘
setInterval(setClock, 1000);

// 初始化時鐘（避免刷新後需要等一秒才更新）
setClock();
```

---

如果還有不清楚的部分，可以隨時告訴我，我會更詳細地解釋！ 😊