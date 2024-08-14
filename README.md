<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bouncing Text Box with Cursor Image and YouTube Audio</title>
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

        .guestbook-box {
            position: fixed;
            bottom: 10px;
            left: 10px;
            background-color: rgba(255, 255, 255, 0.8); /* Semi-transparent background */
            border: 2px solid black; /* Border for visibility */
            padding: 10px;
            cursor: pointer; /* Change cursor to pointer to indicate clickability */
            text-align: center;
            font-size: 18px;
            width: 150px;
        }

        .guestbook-box:hover {
            background-color: lightcoral;
        }

        /* Hidden YouTube player iframe */
        #player {
            position: absolute;
            width: 0;
            height: 0;
            overflow: hidden;
        }
    </style>
</head>
<body>
    <div class="bouncing-text">Welcome to my website!</div>
    <img src="https://i.imgur.com/B5oJFTW.png" alt="Cursor Image" class="cursor-image">
    
    <div class="guestbook-box" onclick="openGuestbook()">Guestbook</div>

    <!-- YouTube Player API script -->
    <script src="https://www.youtube.com/iframe_api"></script>
    <script>
        // Global variable for the player
        let player;

        // Function to create an iframe and YouTube player
        function onYouTubeIframeAPIReady() {
            player = new YT.Player('player', {
                height: '0',
                width: '0',
                videoId: '<iframe width="560" height="315" src="https://www.youtube.com/embed/brwS_ZmVaRc?si=xDLpUdK9KZpXhD92" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>', // Replace with your YouTube video ID
                playerVars: {
                    'autoplay': 1, // Autoplay video
                    'controls': 0, // Hide controls
                    'mute': 1, // Mute the video
                    'loop': 1, // Loop the video
                    'playlist': 'YOUR_VIDEO_ID' // Ensure loop
                },
                events: {
                    'onReady': onPlayerReady
                }
            });
        }

        function onPlayerReady(event) {
            event.target.playVideo();
        }

        function openGuestbook() {
            window.location.href = 'guestbook.html'; // Navigate to the guestbook page
        }

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
    </script>

    <!-- Hidden iframe for YouTube player -->
    <div id="player"></div>
</body>
</html>
