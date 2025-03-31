<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Block Blast - Clone</title>
    <style>
        body { margin: 0; padding: 0; display: flex; justify-content: center; align-items: center; height: 100vh; background-color: #f3f3f3;}
        canvas { border: 2px solid black; }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="300" height="600"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        const grid = [];
        const rows = 12;
        const cols = 10;
        const blockSize = 30;
        let gameOver = false;

        const colors = ["#ff0000", "#00ff00", "#0000ff", "#ffff00", "#ff00ff", "#00ffff"];

        function randomColor() {
            return colors[Math.floor(Math.random() * colors.length)];
        }

        // Initialize the grid
        function createGrid() {
            for (let r = 0; r < rows; r++) {
                grid[r] = [];
                for (let c = 0; c < cols; c++) {
                    grid[r][c] = { color: randomColor(), filled: false };
                }
            }
        }

        // Draw the grid
        function drawGrid() {
            for (let r = 0; r < rows; r++) {
                for (let c = 0; c < cols; c++) {
                    const block = grid[r][c];
                    if (block.filled) {
                        ctx.fillStyle = block.color;
                        ctx.fillRect(c * blockSize, r * blockSize, blockSize, blockSize);
                        ctx.strokeStyle = "#000";
                        ctx.strokeRect(c * blockSize, r * blockSize, blockSize, blockSize);
                    }
                }
            }
        }

        // Check for matching blocks
        function checkMatches() {
            let toClear = [];
            for (let r = 0; r < rows - 1; r++) {
                for (let c = 0; c < cols - 1; c++) {
                    const currentBlock = grid[r][c];
                    if (currentBlock.filled) {
                        if (grid[r + 1][c].filled && grid[r + 1][c].color === currentBlock.color &&
                            grid[r][c + 1].filled && grid[r][c + 1].color === currentBlock.color) {
                            toClear.push({ r, c });
                            toClear.push({ r: r + 1, c });
                            toClear.push({ r, c: c + 1 });
                        }
                    }
                }
            }
            // Clear matched blocks
            toClear.forEach(block => {
                grid[block.r][block.c] = { color: "", filled: false };
            });
        }

        // Drop blocks
        function dropBlocks() {
            for (let c = 0; c < cols; c++) {
                for (let r = rows - 1; r >= 0; r--) {
                    if (!grid[r][c].filled) {
                        for (let k = r - 1; k >= 0; k--) {
                            if (grid[k][c].filled) {
                                grid[r][c] = { ...grid[k][c] };
                                grid[k][c] = { color: "", filled: false };
                                break;
                            }
                        }
                    }
                }
            }
        }

        // Game loop
        function gameLoop() {
            if (gameOver) {
                alert("Game Over!");
                return;
            }

            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawGrid();
            checkMatches();
            dropBlocks();

            requestAnimationFrame(gameLoop);
        }

        // Start the game
        createGrid();
        gameLoop();

    </script>
</body>
</html>
