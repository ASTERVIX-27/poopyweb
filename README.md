<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure Access Portal</title>
    <style>
        /* CSS STYLING */
        body {
            background-color: #121212;
            color: #ffffff;
            font-family: 'Courier New', Courier, monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        .box {
            background-color: #1e1e1e;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            text-align: center;
            width: 350px;
            border: 1px solid #333;
        }

        #code-list {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin: 20px 0;
            font-weight: bold;
            font-size: 1.2rem;
            color: #00ff00; /* Hacker green look */
        }

        input {
            display: block;
            width: 90%;
            padding: 10px;
            margin: 15px auto;
            border: 1px solid #333;
            background-color: #2a2a2a;
            color: white;
            border-radius: 4px;
            text-align: center;
            font-size: 1.1rem;
        }

        button {
            padding: 10px 20px;
            background-color: #222;
            color: #00ff00;
            border: 1px solid #00ff00;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
        }

        button:hover {
            background-color: #00ff00;
            color: #000;
        }

        #status-msg {
            color: #ff3333;
            margin-top: 10px;
        }

        .hidden {
            display: none;
        }

        /* Prank Mode Styles */
        .idiot-mode {
            animation: flash 0.15s infinite;
            text-align: center;
        }

        .idiot-mode h1 {
            font-size: 3.5rem;
            font-weight: bold;
        }

        @keyframes flash {
            0% { background-color: #ffffff; color: #000000; }
            50% { background-color: #000000; color: #ffffff; }
            100% { background-color: #ffffff; color: #000000; }
        }
    </style>
</head>
<body>

    <div id="game-container" class="box">
        <h2>System Verification</h2>
        <p>One of these 10 generated access codes is correct. Guess wisely:</p>
        
        <div id="code-list"></div>

        <form id="guess-form">
            <input type="text" id="user-guess" placeholder="Enter the correct code" maxlength="4" required>
            <button type="submit">Unlock System</button>
        </form>
        <p id="status-msg"></p>
    </div>

    <div id="prank-container" class="hidden">
        <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbWp0cmN4b3g0ZndpZDRvM3N5b3N5cjZndm10bTBwYTA5dW95ZHZ6dyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3o7aD2saalBwwftBIY/giphy.gif" alt="You are an idiot!">
        <h1>YOU ARE AN IDIOT! <br>HAHAHAHAHAHAHA!</h1>
    </div>

    <script>
        // JAVASCRIPT LOGIC
        let codesArray = [];

        // Generate 10 unique 4-digit codes
        while (codesArray.length < 10) {
            let num = Math.floor(1000 + Math.random() * 9000).toString();
            if (!codesArray.includes(num)) {
                codesArray.push(num);
            }
        }

        // Randomly pick one of the 10 to be the winning code
        const correctCode = codesArray[Math.floor(Math.random() * codesArray.length)];

        // Display the 10 codes on the screen
        const listContainer = document.getElementById('code-list');
        codesArray.forEach(code => {
            let codeSpan = document.createElement('span');
            codeSpan.textContent = code;
            listContainer.appendChild(codeSpan);
        });

        // Check the user's guess
        document.getElementById('guess-form').addEventListener('submit', function(event) {
            event.preventDefault();
            
            const userGuess = document.getElementById('user-guess').value.trim();
            const statusMsg = document.getElementById('status-msg');

            if (userGuess === correctCode) {
                // Correct answer! Trigger the prank.
                document.getElementById('game-container').classList.add('hidden');
                document.getElementById('prank-container').classList.remove('hidden');
                document.body.classList.add('idiot-mode');
            } else if (codesArray.includes(userGuess)) {
                // It's one of the listed codes, but not the correct one
                statusMsg.textContent = "Access Denied. Wrong code selection.";
            } else {
                // They typed something completely random
                statusMsg.textContent = "That code isn't even on the screen!";
            }
            
            // Clear the input box for the next attempt
            document.getElementById('user-guess').value = '';
        });
    </script>
</body>
</html>

