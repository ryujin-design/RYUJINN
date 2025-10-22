Hope you like it!!
<html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Couple's Butterfly Haven</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(to bottom, #e0f7ff, #ffffff);
            color: #004080;
            text-align: center;
            margin: 0;
            padding: 20px;
        }
        h1, h2 {
            color: #0066cc;
        }
        .butterfly {
            font-size: 2em;
            color: #99ccff;
        }
        .container {
            max-width: 800px;
            margin: auto;
            background: rgba(255, 255, 255, 0.9);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 102, 204, 0.3);
        }
        input, textarea, button {
            margin: 10px;
            padding: 10px;
            border: 1px solid #99ccff;
            border-radius: 5px;
        }
        button {
            background: #0066cc;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background: #004080;
        }
        canvas {
            border: 1px solid #99ccff;
            background: white;
        }
        #chatMessages {
            height: 200px;
            overflow-y: scroll;
            background: #f0f8ff;
            border: 1px solid #99ccff;
            padding: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🦋 Couple's Butterfly Haven 🦋</h1>
        <p>Welcome to our cozy space! Share pictures, notes, and messages in a world of blue and white butterflies.</p>

        <!-- Login Section -->
        <div id="login">
            <h2>Login</h2>
            <input type="text" id="username" placeholder="Username (couple)">
            <input type="password" id="password" placeholder="Password (love)">
            <button onclick="login()">Enter</button>
        </div>

        <!-- Main Content (Hidden until login) -->
        <div id="main" style="display: none;">
            <h2>🦋 Our Shared Space 🦋</h2>

            <!-- Edit Picture Together -->
            <h3>Draw a Picture Together</h3>
            <p>Use the canvas to doodle butterflies or anything cute!</p>
            <canvas id="drawingCanvas" width="400" height="300"></canvas><br>
            <button onclick="clearCanvas()">Clear Canvas</button>
            <button onclick="saveCanvas()">Save Drawing</button>

            <!-- Letter or Note -->
            <h3>Write a Letter or Note</h3>
            <textarea id="sharedNote" rows="5" cols="50" placeholder="Write your heartfelt note here..."></textarea><br>
            <button onclick="saveNote()">Save Note</button>
            <button onclick="loadNote()">Load Note</button>

            <!-- Type Messages Together -->
            <h3>Chat Together</h3>
            <div id="chatMessages"></div>
            <input type="text" id="messageInput" placeholder="Type a message...">
            <button onclick="sendMessage()">Send</button>
            <button onclick="clearChat()">Clear Chat</button>
        </div>
    </div>

    <script>
        // Simple login (hardcoded for demo)
        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            if (username === 'couple' && password === 'love') {
                document.getElementById('login').style.display = 'none';
                document.getElementById('main').style.display = 'block';
                loadData(); // Load saved data
            } else {
                alert('Invalid credentials!');
            }
        }

        // Drawing Canvas
        const canvas = document.getElementById('drawingCanvas');
        const ctx = canvas.getContext('2d');
        let drawing = false;

        canvas.addEventListener('mousedown', () => drawing = true);
        canvas.addEventListener('mouseup', () => drawing = false);
        canvas.addEventListener('mousemove', draw);

        function draw(e) {
            if (!drawing) return;
            ctx.lineWidth = 2;
            ctx.lineCap = 'round';
            ctx.strokeStyle = '#0066cc';
            ctx.lineTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
        }

        function clearCanvas() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
        }

        function saveCanvas() {
            const dataURL = canvas.toDataURL();
            localStorage.setItem('sharedDrawing', dataURL);
            alert('Drawing saved!');
        }

        // Shared Note
        function saveNote() {
            const note = document.getElementById('sharedNote').value;
            localStorage.setItem('sharedNote', note);
            alert('Note saved!');
        }

        function loadNote() {
            const note = localStorage.getItem('sharedNote') || '';
            document.getElementById('sharedNote').value = note;
        }

        // Chat Messages
        function sendMessage() {
            const input = document.getElementById('messageInput');
            const message = input.value.trim();
            if (message) {
                const chat = document.getElementById('chatMessages');
                chat.innerHTML += `<p><strong>You:</strong> ${message}</p>`;
                input.value = '';
                saveChat();
            }
        }

        function clearChat() {
            document.getElementById('chatMessages').innerHTML = '';
            localStorage.removeItem('sharedChat');
        }

        function saveChat() {
            const chat = document.getElementById('chatMessages').innerHTML;
            localStorage.setItem('sharedChat', chat);
        }

        function loadChat() {
            const chat = localStorage.getItem('sharedChat') || '';
            document.getElementById('chatMessages').innerHTML = chat;
        }

        // Load all data on page load
        function loadData() {
            loadNote();
            loadChat();
            const drawingData = localStorage.getItem('sharedDrawing');
            if (drawingData) {
                const img = new Image();
                img.onload = () => ctx.drawImage(img, 0, 0);
                img.src = drawingData;
            }
        }
    </script>
</body>
</html>
