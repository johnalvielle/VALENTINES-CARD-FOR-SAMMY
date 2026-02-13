# VALENTINES-CARD-FOR-SAMMY

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Love Story Valentine's Invitation for Samantha Reiko</title>
    <style>
        /* Reset and base styles for a soft, dreamy storybook vibe */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Arial', sans-serif; /* Cute, readable font */
            background: linear-gradient(135deg, #ffe4e1 0%, #fff8dc 50%, #f5deb3 100%); /* Pastel pink, cream, beige */
            color: #8b4513; /* Soft brown text */
            overflow-x: hidden;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
            cursor: default; /* Prevent text selection cursor */
        }

        /* Scene containers: Hidden by default, shown via JS */
        .scene {
            display: none; /* Hidden initially */
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            text-align: center;
            animation: sceneFadeIn 2s ease-in-out;
        }
        .scene.active { display: flex; } /* Show active scene */

        /* Scene fade-in animation */
        @keyframes sceneFadeIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }

        /* Typography with soft glow */
        .scene h1, .scene h2, .scene p {
            margin-bottom: 20px;
            text-shadow: 0 0 10px rgba(255, 182, 193, 0.5); /* Gentle pink glow */
        }
        .scene h1 { font-size: 2.5rem; color: #daa520; } /* Warm beige */
        .scene h2 { font-size: 1.8rem; }
        .scene p { font-size: 1.2rem; line-height: 1.6; }

        /* Characters: Bear and Capybara */
        .character {
            font-size: 4rem;
            margin: 10px;
            animation: gentleFloat 3s ease-in-out infinite;
            user-select: none; /* Prevent text selection */
        }
        .bear { color: #daa520; cursor: grab; } /* Beige bear, draggable in Scene 2 */
        .capybara { color: #f5deb3; } /* Cream capybara */
        .bear:active { cursor: grabbing; } /* Dragging cursor */
        @keyframes gentleFloat {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        /* Heart elements */
        .heart {
            font-size: 2rem;
            color: rgba(255, 182, 193, 0.8);
            animation: floatUp 4s linear infinite;
            filter: drop-shadow(0 0 5px rgba(255, 182, 193, 0.5)); /* Sparkle effect */
            cursor: pointer;
            user-select: none;
        }
        .heart.glowing { /* For Scene 3 */
            animation: glow 1s ease-in-out infinite alternate;
        }
        @keyframes floatUp {
            0% { transform: translateY(0) rotate(0deg); }
            100% { transform: translateY(-50px) rotate(360deg); }
        }
        @keyframes glow {
            from { filter: drop-shadow(0 0 5px rgba(255, 182, 193, 0.5)); }
            to { filter: drop-shadow(0 0 15px rgba(255, 182, 193, 1)); }
        }

        /* Buttons in Scene 4 */
        .buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
        }
        button {
            padding: 15px 30px;
            font-size: 1.2rem;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            position: relative;
            user-select: none;
        }
        #yesBtn {
            background: linear-gradient(45deg, #ffe4e1, #fff8dc);
            color: #8b4513;
        }
        #yesBtn:hover { transform: scale(1.1); }
        #noBtn {
            background: linear-gradient(45deg, #fff8dc, #f5deb3);
            color: #8b4513;
        }
        #noBtn:hover { transform: scale(1.05); }

        /* Celebration elements for Scene 5 */
        .celebration {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            overflow: hidden;
        }
        .confetti {
            position: absolute;
            width: 10px;
            height: 10px;
            background: #daa520;
            animation: fall 4s linear forwards;
        }
        .burst-heart {
            font-size: 3rem;
            color: #ffe4e1;
            animation: burst 3s ease-out forwards;
        }
        .hugging-animals {
            font-size: 4rem;
            animation: hug 2s ease-in-out infinite;
        }
        .hugging-animals .bear { color: #daa520; }
        .hugging-animals .capybara { color: #f5deb3; }
        @keyframes fall {
            0% { transform: translateY(-10px) rotate(0deg); opacity: 1; }
            100% { transform: translateY(100vh) rotate(360deg); opacity: 0; }
        }
        @keyframes burst {
            0% { transform: scale(0); opacity: 1; }
            50% { transform: scale(1.5); }
            100% { transform: scale(0); opacity: 0; }
        }
        @keyframes hug {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }

        /* Popup for YES */
        .popup {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            justify-content: center;
            align-items: center;
            z-index: 1000;
            animation: fadeIn 0.5s ease;
        }
        .popup p {
            font-size: 1.5rem;
            background: rgba(255, 255, 255, 0.9);
            color: #daa520;
            padding: 20px;
            border-radius: 20px;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        /* Mobile responsiveness */
        @media (max-width: 768px) {
            .scene h1 { font-size: 2rem; }
            .scene h2 { font-size: 1.5rem; }
            .scene p { font-size: 1rem; }
            .character { font-size: 3rem; }
            .buttons { flex-direction: column; gap: 10px; }
            button { padding: 12px 25px; font-size: 1rem; }
        }
    </style>
</head>
<body>
    <!-- Background music: 100% instrumental, cute kawaii style, soft music box/gentle piano/warm lo-fi, volume 10-15%, starts after first interaction, soft looping -->
    <audio id="bgMusic" muted autoplay loop volume="0.1"> <!-- Volume set to 10% -->
        <!-- Replace with a free 100% instrumental cute kawaii music URL (e.g., soft music box melody from freesound.org or similar). Ensure no vocals/speech/lyrics/sound effects. Placeholder for demo. -->
        <source src="https://www.soundjay.com/misc/sounds/gentle-piano-01.wav" type="audio/wav">
 <!-- Placeholder; user must replace with appropriate instrumental track -->
        Your browser does not support the audio element.
    </audio>

    <!-- Scene 1: First Meeting -->
    <div id="scene1" class="scene active">
        <div class="character bear">🐻</div>
        <div class="character capybara">🦫</div>
        <div class="heart" id="heart1">💖</div>
        <div class="heart" id="heart2">💖</div>
        <div class="heart" id="heart3">💖</div>
        <p>Once upon a moment, two hearts slowly found each other...</p>
        <p id="tapHint">Tap the hearts to continue!</p>
    </div>

    <!-- Scene 2: Growing Closer -->
    <div id="scene2" class="scene">
        <div class="character bear" id="draggableBear">🐻</div>
        <div class="character capybara" id="targetCapybara">🦫</div>
        <div class="heart">💖</div>
        <div class="heart">💖</div>
        <p>Every day felt warmer, softer, and happier...</p>
        <p id="dragHint">Drag the bear to the capybara!</p>
    </div>

    <!-- Scene 3: Love Confession -->
    <div id="scene3" class="scene">
        <div class="character bear">🐻</div>
        <div class="character capybara">🦫</div>
        <div class="heart glowing" id="glowingHeart">💖</div>
        <h1>Samantha Reiko 💖</h1>
        <p>I made this just for you...</p>
        <p>I love you, bebe. 💕</p>
        <p id="clickHint">Click the glowing heart!</p>
    </div>

    <!-- Scene 4: Valentine Invitation -->
    <div id="scene4" class="scene">
        <h2>Will you be my Valentine this February 14?</h2>
        <div class="buttons">
            <button id="yesBtn">YES 💕</button>
            <button id="noBtn">NO 🙈</button>
        </div>
    </div>

    <!-- Scene 5: Celebration Ending -->
    <div id="scene5" class="scene">
        <div class="celebration">
            <!-- Confetti and hearts added via JS -->
        </div>
        <div class="hugging-animals">
            <span class="bear">🐻</span><span class="capybara">🦫</span>
        </div>
        <div id="popup" class="popup">
            <p>Yay Samantha! You just made my heart so happy 💖 I can’t wait to spend Valentine’s Day with you. I love you, bebe!</p>
        </div>
    </div>

    <script>
        // JavaScript for interactive story progression and mini-games

        // Scene management variables
        let currentScene = 1;
        const totalScenes = 5;
        const scenes = document.querySelectorAll('.scene');

        function nextScene() {
            if (currentScene < totalScenes) {
                scenes[currentScene - 1].classList.remove('active');
                currentScene++;
                scenes[currentScene - 1].classList.add('active');
            }
        }

        // Background music: Unmute and start at 10-15% volume on first interaction
        let musicStarted = false;
        function startMusic() {
            if (!musicStarted) {
                const bgMusic = document.getElementById('bgMusic');
                bgMusic.volume = 0.1; // 10% volume
                bgMusic.muted = false;
                bgMusic.play();
                musicStarted = true;
            }
        }

        // Scene 1: Tap hearts 3 times to continue
        let tapCount = 0;
        const hearts = [document.getElementById('heart1'), document.getElementById('heart2'), document.getElementById('heart3')];
        hearts.forEach(heart => {
            heart.addEventListener('click', () => {
                startMusic(); // Start music on interaction
                tapCount++;
                heart.style.opacity = '0.5'; // Visual feedback
                if (tapCount >= 3) {
                    document.getElementById('tapHint').style.display = 'none';
                    setTimeout(nextScene, 500);
                }
            });
        });

        // Scene 2: Drag bear to capybara
        const draggableBear = document.getElementById('draggableBear');
        const targetCapybara = document.getElementById('targetCapybara');
        let isDragging = false;
        let offsetX, offsetY;

        // Mouse events for desktop
        draggableBear.addEventListener('mousedown', (e) => {
            isDragging = true;
            offsetX = e.clientX - draggableBear.getBoundingClientRect().left;
            offsetY = e.clientY - draggableBear.getBoundingClientRect().top;
            draggableBear.style.position = 'absolute';
        });

        document.addEventListener('mousemove', (e) => {
            if (isDragging) {
                draggableBear.style.left = (e.clientX - offsetX) + 'px';
                draggableBear.style.top = (e.clientY - offsetY) + 'px';
            }
        });

        document.addEventListener('mouseup', () => {
            if (isDragging) {
                isDragging = false;
                // Check if bear is near capybara
                const bearRect = draggableBear.getBoundingClientRect();
                const capyRect = targetCapybara.getBoundingClientRect();
                const distance = Math.sqrt((bearRect.left - capyRect.left) ** 2 + (bearRect.top - capyRect.top) ** 2);
                if (distance < 100) { // Proximity threshold
                    document.getElementById('dragHint').style.display = 'none';
                    setTimeout(nextScene, 500);
                }
            }
        });

        // Touch events for mobile
        draggableBear.addEventListener('touchstart', (e) => {
            isDragging = true;
            const touch = e.touches[0];
            offsetX = touch.clientX - draggableBear.getBoundingClientRect().left;
            offsetY = touch.clientY - draggableBear.getBoundingClientRect().top;
            draggableBear.style.position = 'absolute';
        });

        document.addEventListener('touchmove', (e) => {
            if (isDragging) {
                const touch = e.touches[0];
                draggableBear.style.left = (touch.clientX - offsetX) + 'px';
                draggableBear.style.top = (touch.clientY - offsetY) + 'px';
            }
        });

        document.addEventListener('touchend', () => {
            if (isDragging) {
                isDragging = false;
                // Same proximity check as mouse
                const bearRect = draggableBear.getBoundingClientRect();
                const capyRect = targetCapybara.getBoundingClientRect();
                const distance = Math.sqrt((bearRect.left - capyRect.left) ** 2 + (bearRect.top - capyRect.top) ** 2);
                if (distance < 100) {
                    document.getElementById('dragHint').style.display = 'none';
                    setTimeout(nextScene, 500);
                }
            }
        });

        // Scene 3: Click glowing heart to continue
        const glowingHeart = document.getElementById('glowingHeart');
        glowingHeart.addEventListener('click', () => {
            document.getElementById('clickHint').style.display = 'none';
            setTimeout(nextScene, 500);
        });

        // Scene 4: NO button runs away
        const noBtn = document.getElementById('noBtn');
        function moveNoBtn() {
            const maxX = window.innerWidth - noBtn.offsetWidth;
            const maxY = window.innerHeight - noBtn.offsetHeight;
            const newX = Math.random() * maxX;
            const newY = Math.random() * maxY;
            noBtn.style.position = 'fixed';
            noBtn.style.left = newX + 'px';
            noBtn.style.top = newY + 'px';
            noBtn.style.transition = 'all 0.5s ease';
        }
        noBtn.addEventListener('mouseover', moveNoBtn);
        noBtn.addEventListener('click', moveNoBtn);

        // YES button: Trigger celebration
        const yesBtn = document.getElementById('yesBtn');
        const popup = document.getElementById('popup');
        const celebration = document.querySelector('.celebration');

        yesBtn.addEventListener('click', () => {
            // Hide buttons and advance to celebration
            document.querySelector('.buttons').style.display = 'none';
            scenes[3].classList.remove('active'); // Hide Scene 4
            scenes[4].classList.add('active'); // Show Scene 5
            popup.style.display = 'flex';

            // Add confetti
            for (let i = 0; i < 50; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + '%';
                confetti.style.background = `hsl(${Math.random() * 360}, 100%, 50%)`;
                confetti.style.animationDelay = Math.random() * 2 + 's';
                celebration.appendChild(confetti);
                setTimeout
                setTimeout(() => confetti.remove(), 4000);
            }

            // Add burst hearts
            for (let i = 0; i < 20; i++) {
                const burstHeart = document.createElement('div');
                burstHeart.className = 'burst-heart';
                burstHeart.textContent = '💖';
                burstHeart.style.left = Math.random() * 100 + '%';
                burstHeart.style.top = Math.random() * 100 + '%';
                burstHeart.style.animationDelay = Math.random() * 1 + 's';
                celebration.appendChild(burstHeart);
                setTimeout(() => burstHeart.remove(), 3000);
            }
        });

        // Close popup on click
        popup.addEventListener('click', () => {
            popup.style.display = 'none';
        });
    </script>
</body>
</html>
