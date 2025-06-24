<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Bouncing Text Box with Cursor Image and Guestbook</title>
    <style>
        /* Hide GitHub Pages title/header if it shows your name */
        h1, header, .site-header, .page-header, .project-name {
            display: none !important;
        }

        @keyframes cartoonSpinIn {
            0% {
                transform: scale(0) rotate(0deg) translate(-50%, -50%);
                opacity: 0;
            }
            60% {
                transform: scale(1.2) rotate(720deg) translate(-50%, -50%);
                opacity: 1;
            }
            100% {
                transform: scale(1) rotate(720deg) translate(-50%, -50%);
                opacity: 1;
            }
        }

        .cartoon-animate {
            animation: cartoonSpinIn 0.8s ease-out;
        }

        body {
            background-image: url("https://i.imgur.com/1d9tmQg.png");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            margin: 0;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            height: 100vh;
            cursor: none;
            position: relative;
        }

        .bouncing-text {
            font-family: "Comic Sans MS", cursive, sans-serif;
            font-size: 16px;
            white-space: nowrap;
            padding: 5px 10px;
            background-color: pink;
            color: black;
            position: absolute;
            border: 2px solid black;
            cursor: pointer;
        }

        .cursor-image {
            position: absolute;
            width: 50px;
            height: auto;
            pointer-events: none;

            /* Fix background & border */
            background-color: transparent !important;
            border: none !important;
            image-rendering: auto;
        }

        /* Bottom buttons container */
        .bottom-buttons {
            position: fixed;
            bottom: 10px;
            left: 0;
            right: 0;
            display: flex;
            justify-content: center;
            gap: 20px;
            pointer-events: auto;
            z-index: 100;
        }

        .guestbook-box, .link-box, .gif-button {
            background-color: rgba(255, 255, 255, 0.8);
            border: 2px solid black;
            padding: 10px;
            cursor: pointer;
            text-align: center;
            font-size: 18px;
            width: 150px;
            user-select: none;
            box-sizing: border-box;
        }

        .guestbook-box:hover {
            background-color: lightcoral;
        }

        .link-box {
            background-color: lightblue;
        }
        .link-box:hover {
            background-color: skyblue;
        }

        .gif-button {
            background-color: #eb9819;
            color: black;
            font-weight: bold;
        }
        .gif-button:hover {
            background-color: #d17712;
        }

    </style>
</head>
<body>
    <div class="bouncing-text">Click for good luck!</div>

    <!-- Replace src with your transparent monkey cursor PNG path -->
   <img src="Monkeys/monkeycursor.png" alt="Cursor Image" class="cursor-image" />


    <div class="bottom-buttons">
        <div class="guestbook-box" onclick="openGuestbook()">Guestbook</div>
        <div class="gif-button" onclick="showRandomGif()">Monkey</div>
        <div class="link-box" onclick="openLink()">Cat</div>
    </div>

    <!-- Hidden cat image -->
    <img id="cat-image" src="https://i.imgur.com/IIM6kpY.png" alt="Cat Image"
         style="display: none; max-width: 100%; position: fixed; top: 50%; left: 50%;
         transform: translate(-50%, -50%); border: 4px solid black;" />

    <!-- GIF display container -->
    <div id="gif-container" style="display:none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
        border: 4px solid black; background: white; padding: 10px; z-index: 1000;">
        <button onclick="hideGif()" style="display: block; margin-bottom: 8px; cursor:pointer;">Close GIF</button>
        <img id="random-gif" src="" alt="Random GIF" style="max-width: 90vw; max-height: 80vh; display: block;" />
    </div>

    <script>
        const box = document.querySelector('.bouncing-text');
        const cursorImage = document.querySelector('.cursor-image');
        const speed = 2;

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

                posX += velX;
                posY += velY;

                if (posX <= 0 || posX + boxWidth >= windowWidth) {
                    velX = -velX;
                }
                if (posY <= 0 || posY + boxHeight >= windowHeight) {
                    velY = -velY;
                }

                box.style.left = posX + 'px';
                box.style.top = posY + 'px';

                requestAnimationFrame(update);
            }
        }

        posX = Math.random() * (window.innerWidth - boxWidth);
        posY = Math.random() * (window.innerHeight - boxHeight);
        box.style.left = posX + 'px';
        box.style.top = posY + 'px';

        box.addEventListener('click', () => {
            startBouncing();

            const msg = new SpeechSynthesisUtterance("Good luck, bastard!");
            window.speechSynthesis.speak(msg);
        });

        document.addEventListener('mousemove', (event) => {
            cursorImage.style.left = (event.clientX - cursorImage.width / 2) + 'px';
            cursorImage.style.top = (event.clientY - cursorImage.height / 2) + 'px';
        });

        function openGuestbook() {
            window.location.href = 'guestbook.html';
        }

        function openLink() {
            const catImage = document.getElementById('cat-image');

            if (catImage.style.display === 'none' || catImage.style.display === '') {
                catImage.style.display = 'block';
                catImage.classList.remove('cartoon-animate');
                void catImage.offsetWidth;
                catImage.classList.add('cartoon-animate');
            } else {
                catImage.style.display = 'none';
                catImage.classList.remove('cartoon-animate');
            }
        }

        // GIF feature
        const gifs = [
          'Monkeys/spinning-monkey.gif',
          'Monkeys/monky-monkey.gif',
          'Monkeys/monkey-spinning-444hobi.gif',
          'Monkeys/monkey-spinning.gif',
          'Monkeys/mongy-monke.gif',
          'Monkeys/fat-fat-monkey.gif'
        ];

        function showRandomGif() {
            const container = document.getElementById('gif-container');
            const gifImage = document.getElementById('random-gif');
            const randomIndex = Math.floor(Math.random() * gifs.length);
            console.log('Showing GIF:', gifs[randomIndex]); // For debugging
            gifImage.src = gifs[randomIndex];
            container.style.display = 'block';
        }

        function hideGif() {
            const container = document.getElementById('gif-container');
            const gifImage = document.getElementById('random-gif');
            container.style.display = 'none';
            gifImage.src = '';  // stop GIF playing by clearing src
        }
    </script>
</body>
</html>
