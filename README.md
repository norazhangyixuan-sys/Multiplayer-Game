<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multiplayer Horror - Play With Friends</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0c0c0c 0%, #1a1a2e 50%, #16213e 100%);
            color: #e0e0e0;
            min-height: 100vh;
            line-height: 1.6;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            text-align: center;
            padding: 30px 0;
            margin-bottom: 30px;
        }

        h1 {
            font-size: 3rem;
            color: #ff3a3a;
            text-shadow: 0 0 10px rgba(255, 58, 58, 0.5);
            margin-bottom: 10px;
        }

        .subtitle {
            font-size: 1.2rem;
            color: #aaa;
            margin-bottom: 20px;
        }

        .game-section {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        @media (max-width: 768px) {
            .game-section {
                grid-template-columns: 1fr;
            }
        }

        .unity-container {
            background: #111;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
            aspect-ratio: 16/9;
        }

        .unity-container iframe {
            width: 100%;
            height: 100%;
            border: none;
        }

        .multiplayer-panel {
            background: rgba(20, 20, 30, 0.8);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
        }

        .share-section {
            background: rgba(30, 30, 46, 0.7);
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
            text-align: center;
        }

        .share-link {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid #444;
            border-radius: 5px;
            padding: 10px;
            margin: 10px 0;
            word-break: break-all;
            font-family: monospace;
            color: #4dabf7;
        }

        .copy-btn {
            background: #4dabf7;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
            transition: background 0.3s;
        }

        .copy-btn:hover {
            background: #3b99e6;
        }

        .player-list {
            margin: 20px 0;
        }

        .player-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 10px;
            margin: 5px 0;
            border-radius: 5px;
            display: flex;
            align-items: center;
        }

        .player-status {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            margin-right: 10px;
        }

        .status-online {
            background: #20c997;
        }

        .status-ingame {
            background: #ff6b6b;
        }

        .game-controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 20px;
        }

        .control-btn {
            background: linear-gradient(to right, #ff3a3a, #ff6b6b);
            color: white;
            border: none;
            padding: 15px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: transform 0.3s;
        }

        .control-btn:hover {
            transform: translateY(-2px);
        }

        .control-btn.create {
            background: linear-gradient(to right, #28a745, #20c997);
        }

        .qr-code {
            text-align: center;
            margin: 20px 0;
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
        }

        .qr-placeholder {
            width: 150px;
            height: 150px;
            background: #222;
            margin: 0 auto;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
            border-radius: 8px;
        }

        footer {
            text-align: center;
            padding: 20px 0;
            margin-top: 30px;
            border-top: 1px solid #333;
            color: #777;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>MULTIPLAYER HORROR</h1>
            <p class="subtitle">Share this link with friends to play together!</p>
        </header>

        <div class="game-section">
            <div class="unity-container">
                <!-- Unity WebGL Game Embed -->
                <iframe src="https://i.simmer.io/@genericUnityGame/multiplayer-horror" title="Multiplayer Horror Game"></iframe>
                <div style="text-align: center; padding: 10px; background: #111; color: #777;">
                    WebGL Game - Share the link above with friends to play together
                </div>
            </div>

            <div class="multiplayer-panel">
                <div class="share-section">
                    <h3>SHARE WITH FRIENDS</h3>
                    <p>Copy this link and send it to your friends:</p>
                    <div class="share-link" id="gameLink">Loading...</div>
                    <button class="copy-btn" onclick="copyGameLink()">Copy Link</button>
                    <button class="copy-btn" onclick="shareToDiscord()">Share to Discord</button>
                    
                    <div class="qr-code">
                        <h4>MOBILE PLAYERS</h4>
                        <p>Scan QR code to join on mobile:</p>
                        <div class="qr-placeholder">
                            QR Code<br>Generator
                        </div>
                    </div>
                </div>

                <div class="player-list">
                    <h3>PLAYERS ONLINE</h3>
                    <div class="player-item">
                        <div class="player-status status-online"></div>
                        <span>You (Host)</span>
                    </div>
                    <div id="playerList">
                        <!-- Players will appear here -->
                    </div>
                </div>

                <div class="game-controls">
                    <button class="control-btn create" onclick="createGame()">CREATE GAME</button>
                    <button class="control-btn" onclick="joinGame()">QUICK JOIN</button>
                </div>

                <div style="margin-top: 20px; text-align: center;">
                    <h4>INVITE FRIENDS</h4>
                    <button class="copy-btn" onclick="inviteFriends()" style="background: #9c36b5;">INVITE FRIENDS</button>
                </div>
            </div>
        </div>

        <div style="background: rgba(20, 20, 30, 0.8); border-radius: 10px; padding: 20px; margin-top: 20px;">
            <h3>HOW TO PLAY WITH FRIENDS</h3>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-top: 15px;">
                <div style="background: rgba(30, 30, 46, 0.7); padding: 15px; border-radius: 8px;">
                    <h4>1. COPY LINK</h4>
                    <p>Copy the game link above</p>
                </div>
                <div style="background: rgba(30, 30, 46, 0.7); padding: 15px; border-radius: 8px;">
                    <h4>2. SHARE IT</h4>
                    <p>Send to friends via Discord, WhatsApp, etc.</p>
                </div>
                <div style="background: rgba(30, 30, 46, 0.7); padding: 15px; border-radius: 8px;">
                    <h4>3. PLAY TOGETHER</h4>
                    <p>Friends click link and join your game</p>
                </div>
            </div>
        </div>

        <footer>
            <p>Multiplayer Horror Game &copy; 2023 | Share this page to play with friends</p>
            <p>This is a demonstration interface. Friends can join using the same link.</p>
        </footer>
    </div>

    <script>
        // Get the current page URL for sharing
        function updateGameLink() {
            const gameLink = window.location.href;
            document.getElementById('gameLink').textContent = gameLink;
            return gameLink;
        }

        // Copy game link to clipboard
        function copyGameLink() {
            const gameLink = updateGameLink();
            navigator.clipboard.writeText(gameLink).then(() => {
                alert('Game link copied to clipboard! Send this to your friends.');
            }).catch(() => {
                // Fallback for older browsers
                const textArea = document.createElement('textarea');
                textArea.value = gameLink;
                document.body.appendChild(textArea);
                textArea.select();
                document.execCommand('copy');
                document.body.removeChild(textArea);
                alert('Game link copied to clipboard!');
            });
        }

        // Share to Discord
        function shareToDiscord() {
            const gameLink = updateGameLink();
            const discordMessage = `Join my Multiplayer Horror game! ${gameLink}`;
            const discordUrl = `https://discord.com/channels/@me?message=${encodeURIComponent(discordMessage)}`;
            window.open(discordUrl, '_blank');
        }

        // Simulate player joining
        function simulatePlayerJoin() {
            const playerNames = ['ShadowHunter', 'DarkRanger', 'NightWalker', 'Phantom', 'Ghost', 'Spectre', 'Wraith'];
            const randomName = playerNames[Math.floor(Math.random() * playerNames.length)];
            
            const playerList = document.getElementById('playerList');
            const playerItem = document.createElement('div');
            playerItem.className = 'player-item';
            playerItem.innerHTML = `
                <div class="player-status status-ingame"></div>
                <span>${randomName}</span>
            `;
            playerList.appendChild(playerItem);

            // Show notification
            if (Notification.permission === 'granted') {
                new Notification('Player Joined', {
                    body: `${randomName} joined your game`,
                    icon: '/favicon.ico'
                });
            }
        }

        // Create game session
        function createGame() {
            alert('Creating game session...\n\nShare the link above with friends so they can join you!');
            updateGameLink();
            
            // Simulate players joining over time
            setTimeout(simulatePlayerJoin, 2000);
            setTimeout(simulatePlayerJoin, 5000);
            setTimeout(simulatePlayerJoin, 8000);
        }

        // Join game
        function joinGame() {
            alert('Searching for active games...\n\nJoining a random multiplayer session!');
        }

        // Invite friends
        function inviteFriends() {
            const gameLink = updateGameLink();
            if (navigator.share) {
                navigator.share({
                    title: 'Join My Multiplayer Horror Game',
                    text: 'Let\'s play Multiplayer Horror together!',
                    url: gameLink
                }).then(() => {
                    console.log('Thanks for sharing!');
                }).catch(console.error);
            } else {
                copyGameLink();
            }
        }

        // Request notification permission
        if ('Notification' in window && Notification.permission === 'default') {
            Notification.requestPermission();
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            updateGameLink();
            
            // Update player count periodically
            setInterval(() => {
                if (Math.random() > 0.7) {
                    simulatePlayerJoin();
                }
            }, 15000);
        });

        // Generate QR code (simulated)
        function generateQRCode() {
            const qrPlaceholder = document.querySelector('.qr-placeholder');
            const gameLink = updateGameLink();
            qrPlaceholder.innerHTML = `
                <div style="text-align: center;">
                    <div style="font-size: 2rem;">📱</div>
                    <div style="font-size: 0.8rem; margin-top: 5px;">Scan with Phone</div>
                    <div style="font-size: 0.7rem; color: #888;">URL: ${gameLink.split('/')[2]}</div>
                </div>
            `;
        }

        // Initialize QR code
        generateQRCode();
    </script>
</body>
</html>
