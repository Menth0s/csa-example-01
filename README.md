      ______ _______ _______ _______   _______ _______ _______ _______ 
    |   __ \       |    |  |     __| |     __|   _   |   |   |    ___|
    |    __/   -   |       |    |  | |    |  |       |       |    ___|
    |___|  |_______|__|____|_______| |_______|___|___|__|_|__|_______|


## HTML:
```html
<div id="gamebox">
  <h1>Pong Game</h1>
  <canvas id="pong" width="480" height="320"></canvas>
  <p id="fps"></p>
</div>
```

## CSS:
```css
* {
  font-family: monospace;
}

body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

#gamebox {
  width: 480px;
  margin: 0 auto;
}

h1 {
  text-align: center;
}

#fps {
  text-align: right;
}

#pong {
  background-color: #eee;
  display: block;
}
```

### JavaScript:

```javascript
const canvas = document.getElementById("pong");
const ctx = canvas.getContext('2d');

const ball_radius = 10;
let x = canvas.width / 2;
let y = canvas.height - 40;
let dx = 4;
let dy = -4;

const paddle_height = 10;
const paddle_width = 75;
let paddle_x = (canvas.width - paddle_width) / 2;
let paddle_dx = 7;

let right_pressed = false;
let left_pressed = false;

function key_down_handler(event) {
  if (event.keyCode === 39) {
    right_pressed = true;
  } else if (event.keyCode === 37) {
    left_pressed = true;
  }
}

function key_up_handler(event) {
  if (event.keyCode === 39) {
    right_pressed = false;
  } else if (event.keyCode === 37) {
    left_pressed = false;
  }
}
function draw_paddle() {
  ctx.beginPath();
  ctx.rect(
    paddle_x,
    canvas.height - paddle_height,
    paddle_width,
    paddle_height);
  ctx.fillStyle = 'red';
  ctx.fill();
  ctx.closePath();
}

function draw_ball() {
  ctx.beginPath();
  ctx.arc(x, y, ball_radius, 0, Math.PI * 2);
  ctx.fillStyle = 'blue';
  ctx.fill();
  ctx.closePath();
}

function game_loop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  draw_ball();
  draw_paddle();

  if (x + dx > canvas.width - ball_radius || x + dx < ball_radius) {
    dx = -dx;
  }
  if ((y + dy < ball_radius) ||
    (y + dy > canvas.height - ball_radius - paddle_height
      && x + dx > paddle_x
      && x + dx < paddle_x + paddle_width)
  ) {
    dy = -dy;
  }

  if (right_pressed && (paddle_x + paddle_width) < canvas.width) {
    paddle_x += paddle_dx;
  } else if (left_pressed && paddle_x > 0) {
    paddle_x -= paddle_dx;
  } else if (y + dy > canvas.height - ball_radius) {
    alert('Game over!');
    location.reload();
  }

  x += dx;
  y += dy;

  requestAnimationFrame(game_loop);
}

requestAnimationFrame(game_loop);

document.addEventListener('keydown', key_down_handler, false);
document.addEventListener('keyup', key_up_handler, false);
```
