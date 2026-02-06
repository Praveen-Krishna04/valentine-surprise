<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Be My Valentine?</title>
    <style>
        body {
            /* --- BACKGROUND COLOR --- */
            background-color: #ffb6c1; 
            
            margin: 0;
            height: 100vh; /* Full screen height */
            display: flex;
            justify-content: center; /* Centers horizontally */
            align-items: center;     /* Centers vertically */
            font-family: 'Arial', sans-serif;
            overflow: hidden;        /* Prevents scrolling */
        }

        /* The Container for the content */
        .container {
            position: relative;
            text-align: center; 
            width: 100%;
            max-width: 600px;
        }

        h1 {
            color: #d32f2f;
            font-size: 3rem;
            margin-bottom: 30px;
        }

        h2 {
            color: #d32f2f;
            font-size: 2rem;
            margin-bottom: 20px;
        }

        p {
            font-size: 2rem;
            color: #333;
            font-weight: bold;
        }

        /* Button Styling */
        .btn {
            padding: 15px 40px;
            font-size: 22px;
            margin: 10px;
            cursor: pointer;
            border-radius: 50px;
            border: none;
            font-weight: bold;
            color: white;
            transition: transform 0.2s;
            display: inline-block; /* Keeps them side by side */
        }

        #btn-yes {
            background-color: #ff4081; /* Pink */
            box-shadow: 0 5px 15px rgba(255, 64, 129, 0.4);
        }
        
        #btn-yes:hover {
            transform: scale(1.1);
        }

        #btn-no {
            background-color: #9e9e9e; /* Grey */
            position: relative; /* Starts next to the Yes button */
        }

        /* CAPTCHA Grid Styling */
        .captcha-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            max-width: 450px;
            margin: 30px auto;
        }

        .captcha-box {
            width: 130px;
            height: 130px;
            border: 3px solid #d32f2f;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 60px;
            cursor: pointer;
            transition: all 0.3s;
            background-color: white;
        }

        .captcha-box:hover {
            transform: scale(1.05);
            box-shadow: 0 5px 15px rgba(255, 64, 129, 0.3);
        }

        .captcha-box.selected {
            background-color: #ffebee;
            border-color: #ff4081;
            border-width: 5px;
        }

        #captcha-submit {
            background-color: #ff4081;
            box-shadow: 0 5px 15px rgba(255, 64, 129, 0.4);
            margin-top: 20px;
        }

        #captcha-submit:hover {
            transform: scale(1.1);
        }

        #captcha-message {
            color: #d32f2f;
            font-size: 1.2rem;
            margin-top: 15px;
            min-height: 30px;
            font-weight: bold;
        }

        /* Image styling for the success page */
        img {
            max-width: 300px;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }

        /* Utility to hide screens initially */
        .hidden {
            display: none;
        }
    </style>
</head>
<body>

    <!-- SCREEN 1: The Question -->
    <div id="screen-question" class="container">
        <h1>Will You Be My Valentine?</h1>
        <div>
            <button id="btn-yes" class="btn">Yes</button>
            <button id="btn-no" class="btn">Try to say No... we'll see 😏</button>
        </div>
    </div>

    <!-- SCREEN 2: CAPTCHA Challenge (Initially Hidden) -->
    <div id="screen-captcha" class="container hidden">
        <h2>🤖 Prove You're Not a Robot!</h2>
        <p style="font-size: 1.3rem;">Select all the hearts ❤️</p>
        
        <div class="captcha-grid">
            <div class="captcha-box" data-type="heart">❤️</div>
            <div class="captcha-box" data-type="other">🌟</div>
            <div class="captcha-box" data-type="heart">💕</div>
            <div class="captcha-box" data-type="other">🌈</div>
            <div class="captcha-box" data-type="heart">💖</div>
            <div class="captcha-box" data-type="other">⭐</div>
            <div class="captcha-box" data-type="other">🎵</div>
            <div class="captcha-box" data-type="heart">💗</div>
            <div class="captcha-box" data-type="other">🌸</div>
        </div>

        <div id="captcha-message"></div>
        <button id="captcha-submit" class="btn">Verify</button>
    </div>

    <!-- SCREEN 3: The Success Page (Initially Hidden) -->
    <div id="screen-success" class="container hidden">
        <!-- Cat GIF -->
        <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExM3R6eW55b3E2Z2V2eHM5b3p6eW55b3E2Z2V2eHM5b3p6eW55b3E2ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/MDJ9IbxxvDUQM/giphy.gif" alt="Cute Cat">
        <p>Yay! Me Most!</p>
    </div>

    <script>
        const noBtn = document.getElementById("btn-no");
        const yesBtn = document.getElementById("btn-yes");
        const screenQuestion = document.getElementById("screen-question");
        const screenCaptcha = document.getElementById("screen-captcha");
        const screenSuccess = document.getElementById("screen-success");
        const captchaSubmit = document.getElementById("captcha-submit");
        const captchaMessage = document.getElementById("captcha-message");

        let selectedBoxes = [];

        // 1. "No" Button Movement Logic
        noBtn.addEventListener("mouseover", function() {
            const width = window.innerWidth - 150;
            const height = window.innerHeight - 150;

            const newX = Math.random() * width;
            const newY = Math.random() * height;

            noBtn.style.position = "fixed"; 
            noBtn.style.left = newX + "px";
            noBtn.style.top = newY + "px";
        });

        // 2. "Yes" Button Click Logic (Goes to CAPTCHA)
        yesBtn.addEventListener("click", function() {
            screenQuestion.style.display = "none";
            screenCaptcha.style.display = "block";
        });

        // 3. CAPTCHA Box Selection
        const captchaBoxes = document.querySelectorAll(".captcha-box");
        captchaBoxes.forEach(box => {
            box.addEventListener("click", function() {
                this.classList.toggle("selected");
                captchaMessage.textContent = ""; // Clear any error messages
            });
        });

        // 4. CAPTCHA Verification
        captchaSubmit.addEventListener("click", function() {
            const selected = document.querySelectorAll(".captcha-box.selected");
            const hearts = document.querySelectorAll('.captcha-box[data-type="heart"]');
            
            // Check if all hearts are selected and no others
            let allHeartsSelected = true;
            let onlyHeartsSelected = true;

            hearts.forEach(heart => {
                if (!heart.classList.contains("selected")) {
                    allHeartsSelected = false;
                }
            });

            selected.forEach(box => {
                if (box.dataset.type !== "heart") {
                    onlyHeartsSelected = false;
                }
            });

            if (allHeartsSelected && onlyHeartsSelected) {
                // SUCCESS!
                screenCaptcha.style.display = "none";
                screenSuccess.style.display = "block";
                document.body.style.backgroundColor = "#ffebee";
            } else {
                // FAILED
                captchaMessage.textContent = "❌ Oops! Try again!";
                captchaMessage.style.color = "#d32f2f";
                
                // Shake animation
                screenCaptcha.style.animation = "shake 0.5s";
                setTimeout(() => {
                    screenCaptcha.style.animation = "";
                }, 500);
            }
        });
    </script>

    <style>
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }
    </style>

</body>
</html>
