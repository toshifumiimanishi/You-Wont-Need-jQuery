## You Won't Need jQuery

昨今、**JavaScript の API が充実している** ことにより **ブラウザ間の差異を吸収する** という当初の技術選定の理由が弱くなっている。jQuery メソッドと等価なネイティブコードをいくつか掲載する。また、[You Don't Need jQuery](https://github.com/nefe/You-Dont-Need-jQuery) でも多数掲載されている。

### DOM のレディ状態の確認

#### jQuery ver.

```javascript
$(document).ready(function () {
  // ここにコードが入る
});
```

#### 脱 jQuery ver.

```javascript
document.addEventListener('DOMContentLoaded', function () {
  // ここにコードが入る
});
```

### プログラムからのイベント起動

#### jQuery ver.

```javascript
$('.element').trigger('click');
```

#### 脱 jQuery ver.

```javascript
var clickEvent = new Event('clcik');
document.querySelector('.element').dispatchEvent(clickEvent);
```

#### ヘルパー関数

```javascript
function trigger(selector, eventType) {
  document.querySelector(selector).dispatchEvent(new Event(eventType));
}
```

### まだ存在しない要素ターゲットにする

#### jQuery ver.

```javascript
$('.element').on('mouseover', '.element-item', function () {
  // ここに処理を記述する
});
```

#### 脱 jQuery ver.

```javascript
document.querySelector('.element').addEventListener('mouseover', function (event) {
  if (event.target.className === 'element-item') {
    // ここに処理を記述する
  }
});
```

### バインドされたイベントの削除

#### jQuery ver.

```javascript
$('.element-item').off('mouseover')
```

#### 脱 jQuery ver.

```javascript
document.querySelector('.element-item').removeEventListener('mouseover', EventListener)
```
※ EventListener は、イベントターゲットから削除するイベントハンドラー

### 複数要素のイテレーション

#### jQuery ver.

```javascript
$('ul > li').each(function () {
  // ここに順次処理したいコードを記述する
  // 順次処理される要素は `this` に格納される
})
```

#### 脱 jQuery ver.

```javascript
[].forEach.call(document.querySelectorAll('ul > li'), function (element) {
  // ここに順次処理したいコードを記述する
  // 順次処理される要素は `element` に格納される
});
```

#### ES2015+ ver.

```javascript
Array.from(document.querySelectorAll('ul > li'), element => {
  // ここに順次処理したいコードを記述する
  // 順次処理される要素は `element` に格納される
});
```

### クラスの存在確認

#### jQuery ver.

```javascript
$('.element').hasClass('element')
```

#### 脱 jQuery ver.

```javascript
document.querySelector('.element').classList.contains('element');
```

### 要素の表示と非表示

#### jQuery ver.

```javascript
$('.element').show();
$('.element').hide();
```

#### 脱 jQuery ver.

```javascript
document.querySelector('.element').style.display = 'block';
document.querySelector('.element').style.display = 'none';
```

### 要素のインデックス取得

#### jQuery ver.

```javascript
var elements = $('ul > li');
var element = $('.element');
var index = elements.index(element);
```

#### 脱 jQuery ver.

```javascript
var elements = document.querySelectorAll('ul > li');
var element = document.querySelector('.element');
var index = [].slice.call(elements).indexOf(element)
```

#### ES2015+ ver.

```javascript
const elements = document.querySelectorAll('ul > li');
const element = document.querySelector('.element');
const index = Array.from(elements).indexOf(element);
```


### 要素の座標位置の取得

#### jQuery ver.

```javascript
$('.element').offset();
```

#### 脱 jQuery ver.

```javascript
var rect = document.querySelector('.element').getBoundingClientRect();
var offset = {
  top: rect.top + document.body.scrollTop,
  left: rect.left + document.body.scrollLeft
};
```

#### ヘルパー関数

```javascript
function getOffset (el) {
  var rect = document.querySelector(el).getBoundingClientRect();

  return {
    top: rect.top + document.body.scrollTop,
    left: rect.left + document.body.scrollLeft
  };
}
```

### スムーズスクロール

#### jQuery ver.

```javascript
$('.element').click(function() {
  $('html, body').animate({ scrollTop: 0 }, duration);
  return false;
});
```

#### 脱 jQuery ver.

```javascript
document.querySelector('.element').addEventListener('click', function () {
  document.querySelector('html, body').scrollIntoView({behavior: 'smooth'})
});
```

※ `scrollIntoViewOptions` は、現時点（2018/12/11 現在）では一部のブラウザのみサポートされている。サポート状況は[こちら](https://caniuse.com/#search=scrollIntoView)を参照ください。
