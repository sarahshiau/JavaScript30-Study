# JavaScript30-Study
## **DAY01大目標**
製作一個可以用鍵盤按下來播放聲音的鼓機，並伴隨動畫效果。

---

### **學習重點**
1. **事件監聽器**：學會監聽 `keydown` 和 `transitionend`。
2. **DOM 操作**：如何使用 `querySelector` 和 `classList` 操作 HTML 元素。
3. **音效控制**：如何用 JavaScript 播放和控制音頻。

---

### **拆解小目標**
1. **HTML**：建立按鍵結構和對應的聲音文件。
2. **CSS**：為按鍵設定樣式和動畫效果。
3. **JavaScript**：
   - 監聽鍵盤事件。
   - 播放聲音。
   - 添加並移除動畫。

---

## **詳細內容**

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
---

### **3. JavaScript詳細步驟拆解**
1. 偵測鍵盤按鍵事件 (`keydown`)。
2. 找到對應的聲音文件（`<audio>`）和按鍵（`<div>`）。
3. 播放聲音並加上動畫效果。

#### **a. 監聽鍵盤按鍵事件**
第一步是讓 JavaScript 偵測到鍵盤按下的動作，並找到對應的按鍵。
**步驟：**
1. 使用 keydown 事件來監聽鍵盤按鍵。
2. 獲取按下的按鍵對應的 ASCII 碼。

```javascript

window.addEventListener("keydown", (e) => {
  console.log(e.keyCode); // 顯示按下按鍵的 ASCII 碼
});
```
---
#### **b. 播放聲音**
使用 data-key 找到對應的聲音，並播放它。
**步驟：**
1. 找到對應的 <audio> 元素。
2. 使用 audio.play() 播放聲音。
3. 每次播放時，重置聲音到開頭（避免無法連續播放）。

```javascript
function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  // document.querySelector：
    // 這是一個用來選取 HTML 元素的方法。
    // 透過 CSS 選擇器，它會選取第一個符合條件的元素。
  
  // audio[data-key="${e.keyCode}"]：
    // audio：目標是選取 <audio> 元素（代表音效）。
    // [data-key="${e.keyCode}"]：
    // 這是一個屬性選擇器，目標是找出 data-key 屬性的值等於 e.keyCode 的 <audio>。
    // e.keyCode：事件物件中的 keyCode 屬性，表示按下鍵盤按鍵的 ASCII 碼（例如，65 代表字母 A，83 代表字母 S）。

  if (!audio) return; // 如果沒有對應的音效，跳過
  audio.currentTime = 0; // 從頭播放
  audio.play();
}

window.addEventListener("keydown", playSound);
```
---
#### **c. 添加按鍵動畫**
當按下按鍵時，按鍵需要有動畫效果（例如變大或高亮）。
**步驟：**
1. 在鍵盤按下時，為對應的按鍵添加 .playing 類。
2. 在動畫結束後移除 .playing 類。

```javascript

function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  
  if (!audio) return; // 如果沒有對應音效，跳過
  audio.currentTime = 0;
  audio.play();
  key.classList.add("playing"); // 添加動畫
}

function removeTransition(e) {
  if (e.propertyName !== "transform") return; // 過濾非 transform 的動畫
  this.classList.remove("playing"); // 移除動畫
}

const keys = document.querySelectorAll(".key");
keys.forEach((key) =>
  key.addEventListener("transitionend", removeTransition)
);

window.addEventListener("keydown", playSound);

```
---
#### **d. 移除動畫效果**
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
