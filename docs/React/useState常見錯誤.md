
import Highlight from '@site/src/components/helper/HighLight';
import useStateErrorExample from '@site/src/image/react_useState_error.gif';

# React效能優化實戰工作坊筆記 (一)

近期因為上了<a href='https://learn.thisweb.dev'>this.web</a>，React效能優化實戰工作坊的課程。
從課程中解開一些平常開發時遇到會遇到的問題，甚至是不知道的觀念，內容相當豐富。
但是光是聽過去還是有蠻多無法馬上消化的內容XD，所以決定透過筆記與實作來深化觀念。

### Derived State的原則

- 能在render期間計算出來，就不需要使用State

在課程是以時間倒數的function為例，我這邊實作相同的功能。

### <Highlight color="#25c2a0">初始化函數的使用誤區</Highlight>

下面這段程式碼可能會有甚麼問題呢?

```js
  const [timeLeft, setTimeLeft] = useState({
    days: 0,
    hours: 0,
    minutes: 0,
    secs: 0
  });

useEffect(() => {
  const timer = setInterval(() => {
    const updated = calculateTimeLeft(targetTime);
    setTimeLeft(updated);

    // 若時間到，停止 timer
    if (updated.total <= 0) {
      clearInterval(timer);
    }
  }, 1000);

  return () => clearInterval(timer);
}, [targetTime]);

..... skip some code here

```
(Note: gif待調整)
<img src={useStateErrorExample} alt="useState_error_ex" style={{width: 400}} />

可以發現畫面一開始會一閃而過顯示「0 0 0 0」，該如何解決這個問題呢?

### <Highlight color="#25c2a0">解法:利用初始化函數算出初始值</Highlight>

需要預先計算初始->使用初始化函數

初始化函數雖然好用，但也要注意錯誤寫法，尤其是在需要傳參數的情況下。

### 需要傳值的情況

```js
// 方式 1: 正確寫法，使用匿名函數
const [timeLeft, setTimeLeft] = useState(() => calculateTimeLeft(targetTime));

// 方式 2: 錯誤寫法
const [timeLeft, setTimeLeft] = useState(calculateTimeLeft(targetTime));

useEffect(() => {
  const timer = setInterval(() => {
    const updated = calculateTimeLeft(targetTime);
    setTimeLeft(updated);

    // 若時間到，停止 timer
    if (updated.total <= 0) {
      clearInterval(timer);
    }
  }, 1000);

  return () => clearInterval(timer);
}, [targetTime]);

.... skip some code here

```

#### 對照表

針對上述兩種初始化函數的寫法，整理比較。

| 特性       | `useState(() => fn())`） | `useState(fn())` |
| -------- | ------------------------------ | -------------------------- |
| 計算時機     | 僅初始化 render 時才執行               | 每次 render 都會執行一次           |
| 效能       | 高（只算一次）                        | 低（每次都算，即使不用）               |
| 適合場景     | 初始化成本高或動態計算的初始值                | 初始值為常數              |
| React 行為 | React 傳入一個「函數」，會延遲執行它          | React 傳入「值」，會立即執行它   |


### 不需要傳值的情況

單傳傳函數進去就好

```js
const [timeLeft, setTimeLeft] = useState(calculateTimeLeft);

```

### 小結

1. 如果發現畫面閃了不正確的畫面或useState的初始值需要計算，可以聯想到是否需要使用<Highlight color="#25c2a0">初始化函數</Highlight>。
2. 若需要傳參數可以使用匿名函數。

我的實作連結: 
  (1) https://replit.com/@shepherdlord111/Derived-State#src/components/count.jsx (以函數需要傳值為例)
  (2) https://replit.com/@shepherdlord111/Derived-State-example2#src/Calculate.jsx (以函數不需要傳值為例)