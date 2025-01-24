### **JavaScript30 - Day 4: Array Cardio Day 1**

Day 4 的目標是通過對 **陣列的操作**，練習 JavaScript 提供的高階陣列方法（如 `filter`、`map`、`sort`、`reduce` 等），幫助你熟悉常見的陣列處理技巧。

---

## **大目標**
熟悉以下高階陣列方法，學會如何在日常程式中靈活運用它們：
1. `filter()`：篩選陣列。
2. `map()`：對陣列中的每個元素進行操作，返回新陣列。
3. `sort()`：排序陣列。
4. `reduce()`：對陣列進行累計操作，返回單一值（例如：總和）。

---

## **學習重點**

### **1. `filter()`**
#### **作用**：
篩選出符合條件的陣列元素，返回一個新的陣列。

#### **語法**：
```javascript
const result = array.filter(callback(element, index, array));
```

- **`callback`**：
  - 每個元素的條件函數，返回 `true` 的元素會被加入新陣列。
  - 參數：
    - `element`：當前元素。
    - `index`（可選）：當前元素的索引。
    - `array`（可選）：當前被遍歷的陣列。

---

### **2. `map()`**
#### **作用**：
對陣列中的每個元素進行操作，返回一個新的陣列。

#### **語法**：
```javascript
const result = array.map(callback(element, index, array));
```

- **`callback`**：
  - 每個元素的操作函數，返回值會加入新陣列。
  - 參數：
    - `element`：當前元素。
    - `index`（可選）：當前元素的索引。
    - `array`（可選）：當前被遍歷的陣列。

---

### **3. `sort()`**
#### **作用**：
對陣列進行排序，會改變原陣列。

#### **語法**：
```javascript
array.sort(compareFunction(a, b));
```

- **`compareFunction`**：
  - 比較函數，定義元素的排序規則。
  - **返回值**：
    - `負值`：`a` 排在 `b` 前。
    - `正值`：`b` 排在 `a` 前。
    - `0`：順序不變。

---

### **4. `reduce()`**
#### **作用**：
對陣列進行累加操作，最終返回單一值（例如：總和、統計結果）。

#### **語法**：
```javascript
const result = array.reduce(callback(accumulator, currentValue, index, array), initialValue);
```

- **`callback`**：
  - 累計函數。
  - 參數：
    - `accumulator`：累計值。
    - `currentValue`：當前元素的值。
    - `index`（可選）：當前元素的索引。
    - `array`（可選）：當前被遍歷的陣列。
  - **`initialValue`**（可選）：累計的初始值。

---

## **實際練習**

我們將基於以下陣列進行操作：

```javascript
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
  { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
  { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
  { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
  { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
  { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
  { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
  { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
  { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
  { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
  { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 }
];
```

---

### **1. 使用 `filter()`**
#### **篩選出 1500 年代出生的發明家**
```javascript
const fifteen = inventors.filter(inventor => inventor.year >= 1500 && inventor.year < 1600);
console.log(fifteen);
```

- **運行結果**：
  - 篩選出 `year` 在 1500 到 1600 之間的對象。
  - 範例輸出：
    ```javascript
    [
      { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
      { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 }
    ]
    ```

---

### **2. 使用 `map()`**
#### **獲取發明家的全名**
```javascript
const fullNames = inventors.map(inventor => `${inventor.first} ${inventor.last}`);
console.log(fullNames);
```

- **運行結果**：
  - 新陣列只包含發明家的全名。
  - 範例輸出：
    ```javascript
    ["Albert Einstein", "Isaac Newton", "Galileo Galilei", ...]
    ```

---

### **3. 使用 `sort()`**
#### **按出生年份排序（從早到晚）**
```javascript
const ordered = inventors.sort((a, b) => a.year - b.year);
console.log(ordered);
```

- **運行結果**：
  - 按出生年份升序排列的發明家列表。

---

### **4. 使用 `reduce()`**
#### **計算所有發明家活了多少歲**
```javascript
const totalYears = inventors.reduce((total, inventor) => total + (inventor.passed - inventor.year), 0);
console.log(totalYears);
```

- **運行結果**：
  - 計算每位發明家活了多少歲，然後求和。

---

## **延伸練習**

### **1. 結合多個方法**
#### **找出活得最久的發明家**
```javascript
const oldest = inventors
  .map(inventor => ({
    name: `${inventor.first} ${inventor.last}`,
    age: inventor.passed - inventor.year
  }))
  .sort((a, b) => b.age - a.age)[0];

console.log(oldest);
```

- **運行結果**：
  - 找到活得最久的發明家，並返回名字和年齡。

---

### **2. 操作 DOM（選修）**
可以結合這些方法，用於生成動態內容，例如顯示篩選後的列表。

---

## **完整範例**
以下是完整程式碼：
```javascript
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
  { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
  { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
  { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
  { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
  { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
  { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
  { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
  { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
  { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
  { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 }
];

// 篩選出 1500 年代出生的發明家
const fifteen = inventors.filter(inventor => inventor.year >= 1500 && inventor.year < 1600);
console.log('1500 年代出生的發明家:', fifteen);

// 獲取發明家的全名
const fullNames = inventors.map(inventor => `${inventor.first} ${inventor.last}`);
console.log('發明家的全名:', fullNames);

// 按出生年份排序（從早到晚）
const ordered = inventors.sort((a, b) => a.year - b.year);
console.log('按出生年份排序:', ordered);

// 計算所有發明家活了多少歲
const totalYears = inventors.reduce((total, inventor) => total + (inventor.passed - inventor.year), 0);
console.log('發明家總共活了多少年:', totalYears);

// 找出活得最久的發明家
const oldest = inventors
  .map(inventor => ({
    name: `${inventor.first} ${inventor.last}`,
    age: inventor.passed - inventor.year
  }))
  .sort((a, b) => b.age - a.age)[0];
console.log('活得最久的發明家:', oldest);
```

---