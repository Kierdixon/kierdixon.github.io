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
        }

        .guestbook-box {
            position: fixed;
            bottom: 10px;
            left: 10px;
            background-color: rgba(255, 255, 255, 0.8);
            border: 2px solid black;
            padding: 10px;
            cursor: pointer;
            text-align: center;
            font-size: 18px;
            width: 150px;
        }

        .guestbook-box:hover {
            background-color: lightcoral;
        }

        .link-box {
            position: fixed;
            bottom: 10px;
            right: 10px;
            background-color: lightblue;
            border: 2px solid black;
            padding: 10px;
            text-align: center;
            font-size: 18px;
            width: 150px;
            cursor: pointer;
        }

        .link-box:hover {
            background-color: skyblue;
        }
    </style>
</head>
<body>
    <div class="bouncing-text">Alex is a homo!</div>
    <img src="https://i.imgur.com/B5oJFTW.png" alt="Cursor Image" class="cursor-image" />

    <div class="guestbook-box" onclick="openGuestbook()">Guestbook</div>
    <div class="link-box" onclick="openLink()">Cat</div>

    <!-- Hidden cat image -->
    <img id="cat-image" src="https://i.imgur.com/IIM6kpY.png" alt="Cat Image"
         style="display: none; max-width: 100%; position: fixed; top: 50%; left: 50%;
         transform: translate(-50%, -50%); border: 4px solid black;" />

    <!-- SoundCloud Embed -->
    <iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay"
        src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/848674204&color=%236c4068&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true">
    </iframe>
    <div style="font-size: 10px; color: #cccccc; line-break: anywhere; word-break: normal; overflow: hidden; white-space: nowrap; text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif; font-weight: 100;">
        <a href="https://soundcloud.com/mohawkjohnsonmusic" title="Mohawk Johnson" target="_blank" style="color: #cccccc; text-decoration: none;">Mohawk Johnson</a> · 
        <a href="https://soundcloud.com/mohawkjohnsonmusic/slim-jim" title="Slim &amp; Jim" target="_blank" style="color: #cccccc; text-decoration: none;">Slim &amp; Jim</a>
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
    </script>
</body>
</html>
