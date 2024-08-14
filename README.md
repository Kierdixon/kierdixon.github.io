<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bouncing Text Box with Cursor Image and Guestbook</title>
    <style>
        body {
            background-image: url("https://i.imgur.com/1d9tmQg.png");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            margin: 0;
            overflow: hidden; /* Prevent scrollbars from appearing */
            display: flex;
            flex-direction: column;
            height: 100vh;
            cursor: none; /* Hide the default cursor */
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

        .cursor-image {
            position: absolute;
            width: 50px; /* Adjust size as needed */
            height: auto;
            pointer-events: none; /* Ensures that the image doesn't interfere with click events */
        }

        .guestbook {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background-color: rgba(255, 255, 255, 0.8); /* Semi-transparent background */
            border-top: 2px solid black; /* Border for separation */
            padding: 10px;
            box-sizing: border-box;
        }

        .guestbook textarea {
            width: calc(100% - 20px);
            height: 80px;
            margin-bottom: 10px;
            padding: 10px;
            font-size: 16px;
        }

        .guestbook button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: pink;
            border: none;
            cursor: pointer;
        }

        .guestbook button:hover {
            background-color: lightcoral;
        }

        .guestbook .comments {
            margin-top: 10px;
        }

        .guestbook .comments p {
            margin: 5px 0;
            padding: 5px;
            background-color: #f9f9f9;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="bouncing-text">Welcome to my website!</div>
    <img src="https://i.imgur.com/B5oJFTW.png" alt="Cursor Image" class="cursor-image">

    <div class="guestbook">
        <textarea id="commentBox" placeholder="Leave your comment here..."></textarea>
        <button id="submitComment">Submit Comment</button>
        <div class="comments" id="commentsList"></div>
    </div>

    <script>
        const box = document.querySelector('.bouncing-text');
        const cursorImage = document.querySelector('.cursor-image');
        const speed = 2; // Speed of movement in pixels per frame

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

        // Update the position of the cursor image based on mouse movement
        document.addEventListener('mousemove', (event) => {
            cursorImage.style.left = (event.clientX - cursorImage.width / 2) + 'px';
            cursorImage.style.top = (event.clientY - cursorImage.height / 2) + 'px';
        });

        // Guestbook functionality
        const commentBox = document.getElementById('commentBox');
        const submitComment = document.getElementById('submitComment');
        const commentsList = document.getElementById('commentsList');

        function addComment(text) {
            const comment = document.createElement('p');
            comment.textContent = text;
            commentsList.appendChild(comment);
            commentBox.value = ''; // Clear the textarea
        }

        submitComment.addEventListener('click', () => {
            const commentText = commentBox.value.trim();
            if (commentText) {
                addComment(commentText);
            }
        });

        // Optional: Add ability to press "Enter" to submit the comment
        commentBox.addEventListener('keypress', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                submitComment.click();
            }
        });
    </script>
</body>
</html>
