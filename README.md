<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bouncing Text Box</title>
    <style>
        body {
            background-image: url("https://i.imgur.com/1d9tmQg.png");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            margin: 0;
            overflow: hidden; /* Prevent scrollbars from appearing */
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .bouncing-text {
            font-family: "Comic Sans MS", cursive, sans-serif;
            font-size: 16px;
            white-space: nowrap;
            padding: 5px 10px;
            background-color: pink;
            color: black;
            position: absolute;
            border: 2px solid black; /* Optional: add border for visibility */
            cursor: pointer; /* Change cursor to pointer to indicate clickability */
        }
    </style>
</head>
<body>
    <div class="bouncing-text">Welcome to my website!</div>

    <script>
        const box = document.querySelector('.bouncing-text');
        const speed = 1; // Speed of movement in pixels per frame

        let posX = 0;
        let posY = 0;
        let velX = 0;
        let velY = 0;
        const boxWidth = box.offsetWidth;
        const boxHeight = box.offsetHeight;
        let isBouncing = false;

        function startBouncing() {
            if (!isBouncing) {
                isBouncing = true;
                velX = speed;
                velY = speed;
                update();
            }
        }

        function update() {
            if (isBouncing) {
                const windowWidth = window.innerWidth;
                const windowHeight = window.innerHeight;

                // Update position
                posX += velX;
                posY += velY;

                // Check for collision with window edges and reverse direction if needed
                if (posX <= 0 || posX + boxWidth >= windowWidth) {
                    velX = -velX;
                }
                if (posY <= 0 || posY + boxHeight >= windowHeight) {
                    velY = -velY;
                }

                // Apply new position
                box.style.left = posX + 'px';
                box.style.top = posY + 'px';

                requestAnimationFrame(update); // Continue the animation
            }
        }

        // Initialize position
        posX = Math.random() * (window.innerWidth - boxWidth);
        posY = Math.random() * (window.innerHeight - boxHeight);

        // Set initial position of the text box
        box.style.left = posX + 'px';
        box.style.top = posY + 'px';

        // Add click event listener to start bouncing
        box.addEventListener('click', startBouncing);
    </script>
</body>
</html>
