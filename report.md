## Mimic Me! Report

#### simsicon@gmail.com

#### Tasks

1. Display Feature Points
```javascript
function drawFeaturePoints(canvas, img, face) {
  var ctx = canvas.getContext('2d');

  for (var id in face.featurePoints) {
    var featurePoint = face.featurePoints[id];
    ctx.beginPath();
    ctx.strokeStyle = "#EEEEEE"
    ctx.arc(featurePoint.x, featurePoint.y, 3, 0, 2 * Math.PI, true);
    ctx.stroke()
  }
}
```
To display features points, I have looped over the points, then used stroke to draw the circles. I have tried to use `fill()`, but that will cover my face, so I ended up with `storke()`.
2. Show Dominant Emoji
```javascript
function drawEmoji(canvas, img, face) {
  points = Object.values(face.featurePoints)
  emoji_x = Number.NEGATIVE_INFINITY
  emoji_y = Number.POSITIVE_INFINITY
  for (var i = 0 ; i < points.length - 1 ; i ++) {
    if (emoji_x < points[i].x) {
      emoji_x = points[i].x
    }

    if (emoji_y > points[i].y) {
      emoji_y = points[i].y
    }
  }
  ctx.font = "36px Georgia"
  ctx.fillText(face.emojis.dominantEmoji, emoji_x, emoji_y)
}
```
To display the dominant emoji, I have to put the emoji to a relative place, I looped points, to find the right up corner of the face, which is the max of X and min of Y.

3. Implement Mimic Me!
```javascript
function startGame() {
  window.total = 0
  window.correct = 0
  newGame()
}

function newGame() {
  newRound()
  window.interval_id = setInterval(newRound, 10000)
}

function newRound() {
  window.total = window.total + 1
  window.target_emoji = emojis[Math.floor(Math.random() * emojis.length)]
  setTargetEmoji(window.target_emoji)
  setScore(window.correct, window.total)
}

function detectGame(detected_emoji) {
  emoji_code = toUnicode(detected_emoji)

  if (emoji_code == window.target_emoji) {
    window.correct = window.correct + 1
    newRound()
  }
}

function restartGame() {
  clearInterval(window.interval_id)
  window.total = 0
  window.correct = 0
  setScore(window.correct, window.total)
  $("#target").html("?")
  newGame()
}
```
To play the game, I have implemented 5 functions.
Call `startGame()` to initialize a game, and a game will start a timer to limit each round in 15 seconds, if user can not simulate the emoji face, one can not score at that round.
One can call `restartGame()` to play a new game.
