<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secret Music Vault</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            color: white;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            width: 100%;
            max-width: 800px;
            background: rgba(0, 0, 0, 0.7);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            text-align: center;
        }
        
        h1 {
            margin-bottom: 20px;
            font-size: 2.5rem;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }
        
        .subtitle {
            margin-bottom: 30px;
            font-size: 1.2rem;
            opacity: 0.8;
        }
        
        .password-section {
            margin-bottom: 30px;
        }
        
        .hint {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            font-size: 1.1rem;
        }
        
        .password-input {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        input[type="password"] {
            padding: 12px 20px;
            border: none;
            border-radius: 50px;
            width: 70%;
            font-size: 1.1rem;
            background: rgba(255, 255, 255, 0.9);
        }
        
        button {
            padding: 12px 25px;
            border: none;
            border-radius: 50px;
            background: #fdbb2d;
            color: #1a2a6c;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1.1rem;
        }
        
        button:hover {
            background: #ffcc44;
            transform: translateY(-2px);
        }
        
        .message {
            margin-top: 15px;
            min-height: 24px;
            font-weight: bold;
        }
        
        .content-section {
            display: none;
        }
        
        .player {
            margin: 30px 0;
        }
        
        .song-title {
            font-size: 1.8rem;
            margin-bottom: 10px;
        }
        
        .artist {
            font-size: 1.2rem;
            margin-bottom: 20px;
            opacity: 0.8;
        }
        
        .audio-player {
            width: 100%;
            margin: 20px 0;
        }
        
        .lyrics-container {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            max-height: 300px;
            overflow-y: auto;
        }
        
        .lyrics-line {
            margin: 10px 0;
            opacity: 0.5;
            transition: all 0.3s ease;
        }
        
        .lyrics-line.active {
            opacity: 1;
            font-weight: bold;
            font-size: 1.2rem;
            color: #fdbb2d;
            text-shadow: 0 0 10px rgba(253, 187, 45, 0.5);
        }
        
        .hint-button {
            background: transparent;
            border: 2px solid #fdbb2d;
            color: #fdbb2d;
            margin-top: 10px;
        }
        
        .hint-button:hover {
            background: rgba(253, 187, 45, 0.2);
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .password-input {
                flex-direction: column;
                align-items: center;
            }
            
            input[type="password"] {
                width: 100%;
                margin-bottom: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎵 Secret Music Vault 🎵</h1>
        <p class="subtitle">Enter the password to unlock the musical experience</p>
        
        <div class="password-section">
            <div class="hint">
                <p><strong>Hint:</strong> The password is the name of a famous music festival that started in 1969 and shares its name with a small town in New York.</p>
                <button class="hint-button" id="extraHintBtn">Need another hint?</button>
                <div id="extraHint" style="display: none; margin-top: 10px;">
                    <p><strong>Extra Hint:</strong> This festival featured performances by Jimi Hendrix, The Who, and Santana.</p>
                </div>
            </div>
            
            <div class="password-input">
                <input type="password" id="passwordInput" placeholder="Enter password">
                <button id="submitBtn">Unlock</button>
            </div>
            
            <div class="message" id="message"></div>
        </div>
        
        <div class="content-section" id="contentSection">
            <div class="player">
                <h2 class="song-title">Somewhere Over the Rainbow</h2>
                <p class="artist">by Israel Kamakawiwo'ole</p>
                
                <audio controls class="audio-player" id="audioPlayer">
                    <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
                    Your browser does not support the audio element.
                </audio>
                
                <div class="lyrics-container" id="lyricsContainer">
                    <!-- Lyrics will be populated by JavaScript -->
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const passwordInput = document.getElementById('passwordInput');
            const submitBtn = document.getElementById('submitBtn');
            const message = document.getElementById('message');
            const contentSection = document.getElementById('contentSection');
            const audioPlayer = document.getElementById('audioPlayer');
            const lyricsContainer = document.getElementById('lyricsContainer');
            const extraHintBtn = document.getElementById('extraHintBtn');
            const extraHint = document.getElementById('extraHint');
            
            // Correct password
            const correctPassword = "woodstock";
            
            // Lyrics with timings (in seconds)
            const lyrics = [
                { time: 5, text: "Somewhere over the rainbow" },
                { time: 10, text: "Way up high" },
                { time: 15, text: "And the dreams that you dreamed of" },
                { time: 20, text: "Once in a lullaby" },
                { time: 25, text: "Somewhere over the rainbow" },
                { time: 30, text: "Bluebirds fly" },
                { time: 35, text: "And the dreams that you dreamed of" },
                { time: 40, text: "Dreams really do come true" },
                { time: 45, text: "Someday I'll wish upon a star" },
                { time: 50, text: "Wake up where the clouds are far behind me" },
                { time: 55, text: "Where trouble melts like lemon drops" },
                { time: 60, text: "High above the chimney tops" },
                { time: 65, text: "That's where you'll find me" },
                { time: 70, text: "Somewhere over the rainbow" },
                { time: 75, text: "Bluebirds fly" },
                { time: 80, text: "And the dreams that you dared to dream" },
                { time: 85, text: "Really do come true" }
            ];
            
            // Display lyrics
            lyrics.forEach(lyric => {
                const lyricElement = document.createElement('div');
                lyricElement.className = 'lyrics-line';
                lyricElement.textContent = lyric.text;
                lyricElement.dataset.time = lyric.time;
                lyricsContainer.appendChild(lyricElement);
            });
            
            // Password submission
            submitBtn.addEventListener('click', checkPassword);
            passwordInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    checkPassword();
                }
            });
            
            // Extra hint button
            extraHintBtn.addEventListener('click', function() {
                extraHint.style.display = 'block';
            });
            
            // Check password function
            function checkPassword() {
                const enteredPassword = passwordInput.value.toLowerCase().trim();
                
                if (enteredPassword === correctPassword) {
                    message.textContent = "Access granted! Enjoy the music!";
                    message.style.color = "#4CAF50";
                    contentSection.style.display = "block";
                    passwordInput.value = "";
                    
                    // Start playing the song after a short delay
                    setTimeout(() => {
                        audioPlayer.play();
                    }, 1000);
                } else {
                    message.textContent = "Incorrect password. Try again!";
                    message.style.color = "#f44336";
                    contentSection.style.display = "none";
                }
            }
            
            // Lyrics synchronization
            audioPlayer.addEventListener('timeupdate', function() {
                const currentTime = audioPlayer.currentTime;
                
                // Remove active class from all lyrics
                document.querySelectorAll('.lyrics-line').forEach(line => {
                    line.classList.remove('active');
                });
                
                // Find the current lyric based on time
                for (let i = lyrics.length - 1; i >= 0; i--) {
                    if (currentTime >= lyrics[i].time) {
                        const currentLyric = document.querySelector(`.lyrics-line[data-time="${lyrics[i].time}"]`);
                        if (currentLyric) {
                            currentLyric.classList.add('active');
                            
                            // Scroll to keep the active lyric in view
                            currentLyric.scrollIntoView({
                                behavior: 'smooth',
                                block: 'center'
                            });
                        }
                        break;
                    }
                }
            });
        });
    </script>
</body>
</html>
