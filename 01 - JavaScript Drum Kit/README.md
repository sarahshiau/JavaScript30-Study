# JavaScript30-Study
## **最終目標**
製作一個可以用鍵盤按下來播放聲音的鼓機，並伴隨動畫效果。

---

## **項目分解步驟**

### **1. HTML 基礎設置**
在 HTML 中，我們需要準備幾個按鍵和聲音文件。這是我們操作的基礎內容。

#### **a. 準備按鍵和音效**
HTML 的結構大致如下：
```html
<div class="keys">
  <div data-key="65" class="key">
    <kbd>A</kbd>
    <span class="sound">Clap</span>
  </div>
  <div data-key="83" class="key">
    <kbd>S</kbd>
    <span class="sound">HiHat</span>
  </div>
  <!-- 更多按鍵 -->
</div>

<audio data-key="65" src="sounds/clap.wav"></audio>
<audio data-key="83" src="sounds/hihat.wav"></audio>
```

每個 `<div>` 代表一個按鍵，包含：
- **`data-key` 屬性**：對應鍵盤按鍵的 ASCII 碼。
- **聲音文件**：通過 `<audio>` 元素指定按鍵的聲音。

---

### **2. 設定基本樣式**
給按鍵加一些樣式，讓它看起來像按鍵。

#### **CSS 範例**：
```css
.key {
  border: 0.4rem solid black;
  border-radius: 5px;
  padding: 1rem;
  text-align: center;
  margin: 1rem;
  font-size: 1.5rem;
  cursor: pointer;
  transition: all 0.07s ease;
}

.key.playing {
  transform: scale(1.1);
  border-color: yellow;
  box-shadow: 0 0 1rem yellow;
}
```

- 每個按鍵的大小和樣式設定。
- `playing` 類加了動畫效果。

---

### **3. 撰寫 JavaScript**
JavaScript 負責核心邏輯，包括監聽鍵盤事件、播放聲音和添加動畫效果。

#### **a. 偵測鍵盤按鍵**
第一步是讓 JavaScript 偵測到鍵盤按下的動作，並找到對應的按鍵。

```javascript
function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  
  if (!audio) return; // 如果沒有對應音效，直接返回
  audio.currentTime = 0; // 讓音效從頭開始播放
  audio.play(); // 播放音效
  key.classList.add("playing"); // 添加動畫效果
}

window.addEventListener("keydown", playSound);
```

這段程式碼的作用：
1. 偵測鍵盤按鍵事件 (`keydown`)。
2. 找到對應的聲音文件（`<audio>`）和按鍵（`<div>`）。
3. 播放聲音並加上動畫效果。

---

#### **b. 移除動畫效果**
動畫效果應該在短時間內結束。這裡需要監聽動畫的結束事件，然後移除 `playing` 類。

```javascript
function removeTransition(e) {
  if (e.propertyName !== "transform") return; // 過濾掉非 transform 的 transition
  this.classList.remove("playing"); // 移除動畫
}

const keys = document.querySelectorAll(".key");
keys.forEach((key) =>
  key.addEventListener("transitionend", removeTransition)
);
```

這段程式碼的作用：
1. 為每個按鍵監聽 `transitionend` 事件。
2. 在動畫結束時移除 `playing` 類。

---

### **4. 測試與優化**
將 HTML、CSS 和 JavaScript 放在一起，測試專案功能。

#### **完整 JavaScript 程式碼**：
```javascript
function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  
  if (!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add("playing");
}

function removeTransition(e) {
  if (e.propertyName !== "transform") return;
  this.classList.remove("playing");
}

const keys = document.querySelectorAll(".key");
keys.forEach((key) =>
  key.addEventListener("transitionend", removeTransition)
);
window.addEventListener("keydown", playSound);
```

---

### **專案執行步驟總結**
1. **HTML**：建立按鍵結構和對應的聲音文件。
2. **CSS**：為按鍵設定樣式和動畫效果。
3. **JavaScript**：
   - 監聽鍵盤事件。
   - 播放聲音。
   - 添加並移除動畫。

---

### **你可以嘗試的進階挑戰**
1. **新增更多按鍵與聲音**：
   - 添加更多 `<audio>` 和按鍵元素，並測試它們的互動。
2. **改變按鍵觸發方式**：
   - 添加滑鼠點擊觸發的功能（`click` 事件）。
3. **優化樣式**：
   - 調整按鍵的大小、顏色，或添加更酷的動畫效果。

---

### **學習重點**
1. **事件監聽器**：學會監聽 `keydown` 和 `transitionend`。
2. **DOM 操作**：如何使用 `querySelector` 和 `classList` 操作 HTML 元素。
3. **音效控制**：如何用 JavaScript 播放和控制音頻。

---

如果你在實作中遇到具體問題，請告訴我，我可以進一步幫你解釋或協助！😊