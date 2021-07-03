# Whack A Mole

## HTML 代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Whack A Mole!</title>
  <link href='https://fonts.googleapis.com/css?family=Amatic+SC:400,700' rel='stylesheet' type='text/css'>
  <link rel="stylesheet" href="style.css">
</head>

<body>

  <h1>Whack-a-mole! <span class="score">0</span></h1>
  <button onClick="startGame()">Start!</button>

  <div class="game">
    <div class="hole hole1">
      <div class="mole"></div>
    </div>
    <div class="hole hole2">
      <div class="mole"></div>
    </div>
    <div class="hole hole3">
      <div class="mole"></div>
    </div>
    <div class="hole hole4">
      <div class="mole"></div>
    </div>
    <div class="hole hole5">
      <div class="mole"></div>
    </div>
    <div class="hole hole6">
      <div class="mole"></div>
    </div>
  </div>

</body>
</html>
```

- 开始游戏的按钮

```html
 <button onClick="startGame()">Start!</button>
```

- 展示所得分数

```html
<h1>Whack-a-mole! <span class="score">0</span></h1>
```

- 地洞、地鼠

```html
<div class="game">
    <div class="hole hole1">
        <div class="mole"></div>
    </div>
    <div class="hole hole2">
        <div class="mole"></div>
    </div>
    <div class="hole hole3">
        <div class="mole"></div>
    </div>
    <div class="hole hole4">
        <div class="mole"></div>
    </div>
    <div class="hole hole5">
        <div class="mole"></div>
    </div>
    <div class="hole hole6">
        <div class="mole"></div>
    </div>
</div>
```

`hole`为地洞，地洞中有一个地鼠`mole`。

## CSS 代码

```css
html {
  box-sizing: border-box;
  font-size: 10px;
  background: #ffc600;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  padding: 0;
  margin: 0;
  font-family: 'Amatic SC', cursive;
}

h1 {
  text-align: center;
  font-size: 10rem;
  line-height: 1;
  margin-bottom: 0;
}

.score {
  background: rgba(255, 255, 255, 0.2);
  padding: 0 3rem;
  line-height: 1;
  border-radius: 1rem;
}

.game {
  width: 600px;
  height: 400px;
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
}

.hole {
  flex: 1 0 33.33%;
  overflow: hidden;
  position: relative;
}

.hole:after {
  display: block;
  background: url(dirt.svg) bottom center no-repeat;
  background-size: contain;
  content: '';
  width: 100%;
  height: 70px;
  position: absolute;
  z-index: 2;
  bottom: -30px;
}

.mole {
  background: url('mole.svg') bottom center no-repeat;
  background-size: 60%;
  position: absolute;
  top: 100%;
  width: 100%;
  height: 100%;
  transition: all 0.4s;
}

.hole.up .mole {
  top: 0;
}
```

- `dirt.svg`为地洞的图片

- `mole.svg`为地鼠的图片

## JS代码

```js
  <script>
    const holes = document.querySelectorAll('.hole');
    const scoreBoard = document.querySelector('.score');
    const moles = document.querySelectorAll('.mole');
    let lastHole;
    let timeUp = false;
    let score = 0;

    function randomTime(min, max) {
      return Math.round(Math.random() * (max - min) + min);
    }

    function randomHole(holes) {
      const idx = Math.floor(Math.random() * holes.length);
      const hole = holes[idx];
      if (hole === lastHole) {
        return randomHole(holes);
      }
      lastHole = hole;
      return hole;
    }

    function peep() {
      const time = randomTime(200, 1000);
      const hole = randomHole(holes);
      hole.classList.add('up');
      setTimeout(() => {
        hole.classList.remove('up');
        if (!timeUp) peep();
      }, time);
    }

    function startGame() {
      scoreBoard.textContent = 0;
      timeUp = false;
      score = 0;
      peep();
      setTimeout(() => timeUp = true, 10000)
    }

    function bonk(e) {
      score++;
      this.parentNode.classList.remove('up');
      scoreBoard.textContent = score;
    }

    moles.forEach(mole => mole.addEventListener('click', bonk));
  </script>
```

- 获取所有的地洞、得分、地鼠标签

```js
const holes = document.querySelectorAll('.hole');
const scoreBoard = document.querySelector('.score');
const moles = document.querySelectorAll('.mole');
```

- 初始化变量

```js
let lastHole; //用于存储上一次的地洞，主要用于和下一次做对比
let timeUp = false; //判断时间是否结束
let score = 0; //记录得分
```

- 事件监听

```js
// 敲中地鼠，调用bonk方法
moles.forEach(mole => mole.addEventListener('click', bonk));
```

- isTrusted属性

```js
function bonk(e) {
     score++; // 得一分
     this.parentNode.classList.remove('up'); // 隐藏地鼠
     scoreBoard.textContent = score; //更新得分
}
```


- 开始游戏

```js
function startGame() {
    scoreBoard.textContent = 0; //清空得分
    timeUp = false; //将时间是否结束标志设置为false
    score = 0; //得分归零
    peep(); //调用peep()函数
    setTimeout(() => timeUp = true, 10000) //设置倒计时
}
```

- randomTime设置一个`min - max`之间的随机数

[Math.round](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round)

```js
Math.round( 20.49); //  20
Math.round( 20.5);  //  21
Math.round( 42  );  //  42
Math.round(-20.5);  // -20
Math.round(-20.51); // -21
```

```js
function randomTime(min, max) {
    //Math.round() 函数返回一个数字四舍五入后最接近的整数值。
    //Math.random() 函数返回一个浮点,  伪随机数在范围[0，1)，然后您可以缩放到所需的范围。
    return Math.round(Math.random() * (max - min) + min);
}
```

- randomHole函数

[Math.floor](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)

```js
Math.floor( 45.95); //  45
Math.floor( 45.05); //  45
Math.floor(  4   ); //   4
Math.floor(-45.05); // -46 
Math.floor(-45.95); // -46
```

```js
//随机选出一个地洞，用于弹出地鼠
function randomHole(holes) {
    //Math.floor() 返回小于或等于一个给定数字的最大整数。
    const idx = Math.floor(Math.random() * holes.length);
    const hole = holes[idx];
    if (hole === lastHole) {
        return randomHole(holes);
    }
    lastHole = hole;
    return hole;
}
```


- peep()函数

```js
// 地鼠、地洞处理函数
function peep() {
    //产生一个随机数，这个数主要用于设置当前的地鼠什么时候消失
    const time = randomTime(200, 1000);
    //随机产生一个 0 - 5的值，用于随机选择一个地洞
    const hole = randomHole(holes);
    //弹出地鼠
    hole.classList.add('up');
    //time时间后地鼠消失
    setTimeout(() => {
        hole.classList.remove('up');
        //如果倒计时没到，继续调用peep()函数
        if (!timeUp) 
            peep();
    }, time);
}
```
