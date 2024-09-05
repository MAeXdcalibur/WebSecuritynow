<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Security Demo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            margin-bottom: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            background-color: #f9f9f9;
        }
        input, button {
            padding: 8px;
            margin: 5px 0;
            width: calc(100% - 18px);
        }
        #result {
            margin-top: 10px;
        }
        .color-box {
            width: 40px;
            height: 40px;
            display: inline-block;
            margin-right: 5px;
        }
    </style>
</head>
<body>
    <h1>Security Demo</h1>

    <!-- Password Strength Checker -->
    <div class="container">
        <h2>Password Strength Checker</h2>
        <input type="text" id="password" placeholder="Enter password">
        <button onclick="checkPassword()">Check Password</button>
        <div id="result"></div>
    </div>

    <!-- Password Generator -->
    <div class="container">
        <h2>Password Generator</h2>
        <input type="number" id="length" placeholder="Password length" min="8">
        <button onclick="generatePasswords()">Generate Passwords</button>
        <div id="passwords"></div>
    </div>

    <!-- URL Challenge -->
    <div class="container">
        <h2>URL Challenge</h2>
        <a href="#" id="challengeUrl">http://example.com/?access_token=hidden</a>
        <button onclick="showChallenge()">Reveal Data</button>
        <div id="challengeResult"></div>
    </div>

    <!-- Webcam Color Identifier -->
    <div class="container">
        <h2>Webcam Color Identifier</h2>
        <button onclick="startWebcam()">Start Webcam</button>
        <video id="webcam" width="320" height="240" autoplay></video>
        <div id="colorResult"></div>
    </div>

    <script>
        // Password Strength Checker
        function checkPassword() {
            const password = document.getElementById('password').value;
            const result = document.getElementById('result');

            if (password.length < 8) {
                result.textContent = 'Weak Password';
            } else if (/^[a-zA-Z0-9!@#$%^&*()_+]{8,}$/.test(password)) {
                result.textContent = 'Strong Password';
            } else {
                result.textContent = 'Moderate Password';
            }
        }

        // Password Generator
        function generatePasswords() {
            const length = parseInt(document.getElementById('length').value) || 12;
            const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()";
            let passwords = [];

            for (let i = 0; i < 10; i++) {
                let password = "";
                for (let j = 0; j < length; j++) {
                    password += charset.charAt(Math.floor(Math.random() * charset.length));
                }
                passwords.push(password);
            }

            document.getElementById('passwords').innerHTML = passwords.join('<br>');
        }

        // URL Challenge
        function showChallenge() {
            document.getElementById('challengeResult').textContent = 'The hidden data is: "SecretData123"';
        }

        // Webcam Color Identifier
        function startWebcam() {
            const video = document.getElementById('webcam');
            const colorResult = document.getElementById('colorResult');
            const colors = ['red', 'green', 'blue', 'yellow', 'black', 'white', 'orange', 'purple', 'brown'];

            navigator.mediaDevices.getUserMedia({ video: true })
                .then(stream => {
                    video.srcObject = stream;
                    video.onloadedmetadata = () => {
                        video.play();
                        identifyColor();
                    };
                })
                .catch(error => console.error('Error accessing webcam:', error));

            function identifyColor() {
                const canvas = document.createElement('canvas');
                const ctx = canvas.getContext('2d');
                canvas.width = video.videoWidth;
                canvas.height = video.videoHeight;
                ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

                const imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
                const data = imgData.data;
                let colorCounts = colors.reduce((acc, color) => { acc[color] = 0; return acc; }, {});

                for (let i = 0; i < data.length; i += 4) {
                    const r = data[i];
                    const g = data[i + 1];
                    const b = data[i + 2];
                    const color = getColor(r, g, b);
                    if (color) colorCounts[color]++;
                }

                let resultHtml = '';
                colors.forEach(color => {
                    const percentage = ((colorCounts[color] / (data.length / 4)) * 100).toFixed(2);
                    resultHtml += `<div class="color-box" style="background-color: ${color};"></div> ${color}: ${percentage}%<br>`;
                });

                colorResult.innerHTML = resultHtml;

                setTimeout(identifyColor, 2000); // Update every 2 seconds
            }

            function getColor(r, g, b) {
                if (r > 150 && g < 100 && b < 100) return 'red';
                if (r < 100 && g > 150 && b < 100) return 'green';
                if (r < 100 && g < 100 && b > 150) return 'blue';
                if (r > 150 && g > 150 && b < 100) return 'yellow';
                if (r < 100 && g < 100 && b < 100) return 'black';
                if (r > 200 && g > 200 && b > 200) return 'white';
                if (r > 200 && g < 100 && b < 100) return 'orange';
                if (r > 150 && g < 100 && b > 150) return 'purple';
                if (r > 100 && g < 100 && b < 100) return 'brown';
                return null;
            }
        }
    </script>
</body>
</html>
