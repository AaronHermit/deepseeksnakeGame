Creating a simple Snake game using HTML, CSS, and JavaScript is a fun project! Below is a basic implementation of the classic Snake game where the snake grows as it eats food.

### HTML (index.html)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Snake Game</h1>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script src="script.js"></script>
</body>
</html>
```

### CSS (style.css)
```css
body {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
    background-color: #000;
    color: #fff;
    font-family: Arial, sans-serif;
}

canvas {
    border: 1px solid #fff;
    background-color: #000;
}
```

### JavaScript (script.js)
```javascript
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

const gridSize = 20;
const tileCount = canvas.width / gridSize;

let snake = [{ x: 10, y: 10 }];
let direction = { x: 0, y: 0 };
let food = { x: 5, y: 5 };
let score = 0;

function gameLoop() {
    update();
    draw();
    setTimeout(gameLoop, 100);
}

function update() {
    const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

    // Wrap around the screen
    if (head.x < 0) head.x = tileCount - 1;
    if (head.x >= tileCount) head.x = 0;
    if (head.y < 0) head.y = tileCount - 1;
    if (head.y >= tileCount) head.y = 0;

    // Check for collision with itself
    if (snake.some(segment => segment.x === head.x && segment.y === head.y)) {
        resetGame();
        return;
    }

    snake.unshift(head);

    // Check if snake eats the food
    if (head.x === food.x && head.y === food.y) {
        score++;
        placeFood();
    } else {
        snake.pop();
    }
}

function draw() {
    // Clear the canvas
    ctx.fillStyle = "#000";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Draw the snake
    ctx.fillStyle = "lime";
    snake.forEach(segment => ctx.fillRect(segment.x * gridSize, segment.y * gridSize, gridSize, gridSize));

    // Draw the food
    ctx.fillStyle = "red";
    ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize, gridSize);

    // Draw the score
    ctx.fillStyle = "white";
    ctx.font = "20px Arial";
    ctx.fillText("Score: " + score, 10, 30);
}

function placeFood() {
    food.x = Math.floor(Math.random() * tileCount);
    food.y = Math.floor(Math.random() * tileCount);

    // Ensure food doesn't spawn on the snake
    if (snake.some(segment => segment.x === food.x && segment.y === food.y)) {
        placeFood();
    }
}

function resetGame() {
    snake = [{ x: 10, y: 10 }];
    direction = { x: 0, y: 0 };
    score = 0;
    placeFood();
}

document.addEventListener("keydown", event => {
    switch (event.key) {
        case "ArrowUp":
            if (direction.y === 0) direction = { x: 0, y: -1 };
            break;
        case "ArrowDown":
            if (direction.y === 0) direction = { x: 0, y: 1 };
            break;
        case "ArrowLeft":
            if (direction.x === 0) direction = { x: -1, y: 0 };
            break;
        case "ArrowRight":
            if (direction.x === 0) direction = { x: 1, y: 0 };
            break;
    }
});

placeFood();
gameLoop();
```

### How It Works:
1. **HTML**: The `<canvas>` element is used to draw the game.
2. **CSS**: Basic styling to center the game and set the background color.
3. **JavaScript**:
   - The snake is represented as an array of segments.
   - The game loop updates the snake's position and checks for collisions.
   - The `draw` function renders the snake, food, and score on the canvas.
   - The `placeFood` function randomly places the food on the grid.
   - The `resetGame` function resets the game when the snake collides with itself.
   - Keyboard events control the snake's direction.

### How to Run:
1. Save the HTML, CSS, and JavaScript code in their respective files (`index.html`, `style.css`, `script.js`).
2. Open `index.html` in a web browser.
3. Use the arrow keys to control the snake.

Enjoy your simple Snake game! You can expand it by adding features like levels, speed increases, or obstacles.
