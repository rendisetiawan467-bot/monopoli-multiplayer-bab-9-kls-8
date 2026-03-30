# monopoli-multiplayer-bab-9-kls-8
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Monopoli PAK Kelas 8 - Multiplayer Online</title>
    <script src="https://unpkg.com/peerjs@1.4.7/dist/peerjs.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Fredoka:wght@300;400;600;700&family=Nunito:wght@400;600;700&display=swap');
        
        :root {
            --primary: #6366f1;
            --secondary: #8b5cf6;
            --accent: #f59e0b;
            --success: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --info: #3b82f6;
            --dark: #1e293b;
            --light: #f8fafc;
        }
        
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Nunito', sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .header {
            text-align: center;
            padding: 30px 20px;
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            margin-bottom: 30px;
            border: 1px solid rgba(255,255,255,0.2);
        }
        
        .header h1 {
            font-family: 'Fredoka', sans-serif;
            font-size: 2.5rem;
            color: #fff;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 10px;
        }
        
        .header p { color: rgba(255,255,255,0.9); font-size: 1.1rem; }
        
        .connection-status {
            display: inline-block;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.9rem;
            font-weight: 700;
            margin-top: 10px;
        }
        
        .status-connecting { background: #f59e0b; color: white; }
        .status-connected { background: #10b981; color: white; }
        .status-host { background: #3b82f6; color: white; }
        .status-client { background: #8b5cf6; color: white; }
        
        /* Menu Screens */
        .menu-screen {
            background: rgba(255,255,255,0.95);
            border-radius: 30px;
            padding: 40px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            max-width: 600px;
            margin: 0 auto;
            text-align: center;
        }
        
        .menu-title {
            font-family: 'Fredoka', sans-serif;
            font-size: 2rem;
            color: var(--dark);
            margin-bottom: 20px;
        }
        
        .room-code-display {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            border-radius: 20px;
            margin: 20px 0;
            font-size: 3rem;
            font-weight: bold;
            letter-spacing: 10px;
            font-family: monospace;
            box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
        }
        
        .code-input {
            width: 100%;
            padding: 20px;
            font-size: 2rem;
            text-align: center;
            border: 3px solid #e2e8f0;
            border-radius: 15px;
            margin: 20px 0;
            font-family: monospace;
            letter-spacing: 5px;
            text-transform: uppercase;
        }
        
        .code-input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
        }
        
        .btn {
            padding: 15px 40px;
            border: none;
            border-radius: 50px;
            font-family: 'Fredoka', sans-serif;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin: 10px;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
        }
        
        .btn-success {
            background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
            color: white;
            box-shadow: 0 10px 30px rgba(67, 233, 123, 0.4);
        }
        
        .btn-danger {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }
        
        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .players-list {
            background: #f8fafc;
            border-radius: 15px;
            padding: 20px;
            margin: 20px 0;
            text-align: left;
        }
        
        .player-item {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 12px;
            margin: 8px 0;
            background: white;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }
        
        .player-color {
            width: 30px;
            height: 30px;
            border-radius: 50%;
        }
        
        .waiting-text {
            color: #64748b;
            font-style: italic;
            margin: 20px 0;
        }
        
        /* Game Board */
        .game-board {
            display: none;
            background: rgba(255,255,255,0.95);
            border-radius: 30px;
            padding: 30px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
        }
        
        .board-container {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 10px;
            margin-bottom: 30px;
        }
        
        .cell {
            aspect-ratio: 1;
            border-radius: 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 10px;
            text-align: center;
            position: relative;
            overflow: hidden;
            font-size: 0.75rem;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
        }
        
        .cell-start { background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%); color: white; }
        .cell-question { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; }
        .cell-challenge { background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); color: white; }
        .cell-blessing { background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); color: white; }
        .cell-trap { background: linear-gradient(135deg, #fa709a 0%, #fee140 100%); color: white; }
        .cell-finish { background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%); color: var(--dark); }
        
        .cell-number {
            position: absolute;
            top: 5px;
            left: 8px;
            font-weight: bold;
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        .cell-icon { font-size: 1.5rem; margin-bottom: 5px; }
        .cell-title { font-weight: 700; font-size: 0.7rem; line-height: 1.2; }
        
        .player-token {
            position: absolute;
            width: 25px;
            height: 25px;
            border-radius: 50%;
            border: 2px solid white;
            box-shadow: 0 2px 10px rgba(0,0,0,0.3);
            transition: all 0.5s ease;
            z-index: 20;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.7rem;
            font-weight: bold;
            color: white;
        }
        
        .game-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .info-panel {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4e8ec 100%);
            border-radius: 20px;
            padding: 20px;
        }
        
        .info-title {
            font-family: 'Fredoka', sans-serif;
            font-size: 1.3rem;
            color: var(--dark);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .current-player {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        
        .player-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            color: white;
            font-weight: bold;
        }
        
        .player-stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 10px;
        }
        
        .stat-card {
            background: white;
            padding: 12px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            gap: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }
        
        .stat-color {
            width: 30px;
            height: 30px;
            border-radius: 50%;
        }
        
        .stat-name {
            font-weight: 700;
            color: var(--dark);
            font-size: 0.9rem;
        }
        
        .stat-score { font-size: 0.8rem; color: #64748b; }
        
        .dice-area {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            padding: 30px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 20px;
            color: white;
        }
        
        .dice-container { display: flex; gap: 20px; }
        
        .dice {
            width: 80px;
            height: 80px;
            background: white;
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            color: var(--dark);
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            transition: all 0.3s ease;
        }
        
        .dice.rolling { animation: roll 0.5s ease infinite; }
        
        @keyframes roll {
            0%, 100% { transform: rotate(0deg); }
            25% { transform: rotate(90deg); }
            50% { transform: rotate(180deg); }
            75% { transform: rotate(270deg); }
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        
        .modal.active { display: flex; }
        
        .modal-content {
            background: white;
            border-radius: 30px;
            padding: 40px;
            max-width: 600px;
            width: 100%;
            max-height: 80vh;
            overflow-y: auto;
            animation: modalIn 0.3s ease;
        }
        
        @keyframes modalIn {
            from { opacity: 0; transform: scale(0.8) translateY(50px); }
            to { opacity: 1; transform: scale(1) translateY(0); }
        }
        
        .modal-header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .modal-icon { font-size: 4rem; margin-bottom: 15px; }
        
        .modal-title {
            font-family: 'Fredoka', sans-serif;
            font-size: 1.8rem;
            color: var(--dark);
        }
        
        .question-text {
            font-size: 1.2rem;
            color: var(--dark);
            line-height: 1.6;
            margin-bottom: 25px;
            padding: 20px;
            background: linear-gradient(135deg, #f5f7fa 0%, #e4e8ec 100%);
            border-radius: 15px;
            border-left: 5px solid var(--primary);
        }
        
        .options { display: grid; gap: 12px; }
        
        .option {
            padding: 15px 20px;
            background: white;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1rem;
        }
        
        .option:hover {
            border-color: var(--primary);
            background: rgba(99, 102, 241, 0.05);
            transform: translateX(5px);
        }
        
        .option.correct {
            border-color: var(--success);
            background: rgba(16, 185, 129, 0.1);
        }
        
        .option.wrong {
            border-color: var(--danger);
            background: rgba(239, 68, 68, 0.1);
        }
        
        .challenge-box {
            padding: 25px;
            background: linear-gradient(135deg, #fef3c7 0%, #fde68a 100%);
            border-radius: 15px;
            margin-bottom: 20px;
            border: 2px dashed var(--warning);
        }
        
        .challenge-text {
            font-size: 1.1rem;
            color: var(--dark);
            line-height: 1.6;
        }
        
        .result-message {
            text-align: center;
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
            font-weight: 700;
            font-size: 1.1rem;
        }
        
        .result-success {
            background: rgba(16, 185, 129, 0.1);
            color: var(--success);
            border: 2px solid var(--success);
        }
        
        .result-fail {
            background: rgba(239, 68, 68, 0.1);
            color: var(--danger);
            border: 2px solid var(--danger);
        }
        
        .winner-screen {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            z-index: 2000;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            text-align: center;
            padding: 40px;
        }
        
        .winner-screen.active { display: flex; }
        
        .trophy {
            font-size: 8rem;
            animation: bounce 2s ease infinite;
            margin-bottom: 30px;
        }
        
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-30px); }
        }
        
        .winner-title {
            font-family: 'Fredoka', sans-serif;
            font-size: 3rem;
            color: white;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        
        .winner-name {
            font-size: 2rem;
            color: #fde047;
            margin-bottom: 40px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        
        @media (max-width: 768px) {
            .board-container { grid-template-columns: repeat(4, 1fr); }
            .game-info { grid-template-columns: 1fr; }
            .header h1 { font-size: 1.8rem; }
            .room-code-display { font-size: 2rem; letter-spacing: 5px; }
        }
        
        .pulse { animation: pulse 2s ease infinite; }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <div class="header">
            <h1>✝️ Monopoli PAK Kelas 8 - Multiplayer Online</h1>
            <p>Hidup dalam Nilai-nilai Kristiani - Main bersama teman!</p>
            <div id="connectionStatus"></div>
        </div>
        
        <!-- Main Menu -->
        <div class="menu-screen" id="mainMenu">
            <h2 class="menu-title">🎮 Pilih Mode Permainan</h2>
            <p style="color: #64748b; margin-bottom: 30px;">
                Mainkan bersama teman di kelas yang berbeda atau rumah yang berbeda menggunakan kode room!
            </p>
            <button class="btn btn-primary" onclick="showCreateRoom()">🏠 Buat Room Baru</button>
            <button class="btn btn-success" onclick="showJoinRoom()">🔗 Gabung Room</button>
            <button class="btn" onclick="startLocalGame()" style="background: #64748b; color: white;">👤 Main Offline</button>
        </div>
        
        <!-- Create Room Screen -->
        <div class="menu-screen" id="createRoomScreen" style="display: none;">
            <h2 class="menu-title">🏠 Buat Room Baru</h2>
            <p>Bagikan kode ini kepada teman-temanmu:</p>
            <div class="room-code-display" id="hostCode">LOADING...</div>
            <p class="waiting-text" id="waitingText">Menunggu pemain lain bergabung...</p>
            
            <div class="players-list" id="connectedPlayers">
                <h3 style="margin-bottom: 15px;">👥 Pemain Terhubung:</h3>
                <div id="playersListContent"></div>
            </div>
            
            <button class="btn btn-success" id="startGameBtn" onclick="hostStartGame()" disabled>
                🎲 Mulai Permainan (Butuh minimal 2 pemain)
            </button>
            <button class="btn btn-danger" onclick="location.reload()">❌ Batal</button>
        </div>
        
        <!-- Join Room Screen -->
        <div class="menu-screen" id="joinRoomScreen" style="display: none;">
            <h2 class="menu-title">🔗 Gabung Room</h2>
            <p>Masukkan kode room dari temanmu:</p>
            <input type="text" class="code-input" id="joinCodeInput" placeholder="ABCD1234" maxlength="8">
            <input type="text" class="player-input" id="joinPlayerName" placeholder="Nama Kamu" style="width: 100%; padding: 15px; font-size: 1.2rem; border: 2px solid #e2e8f0; border-radius: 10px; margin: 10px 0;">
            <button class="btn btn-success" onclick="joinRoom()">🚀 Gabung Permainan</button>
            <button class="btn btn-danger" onclick="location.reload()">❌ Kembali</button>
        </div>
        
        <!-- Game Board -->
        <div class="game-board" id="gameBoard">
            <div class="board-container" id="boardContainer"></div>
            
            <div class="game-info">
                <div class="info-panel">
                    <div class="info-title">🎯 Giliran Bermain</div>
                    <div class="current-player" id="currentPlayer"></div>
                    <div id="yourTurnIndicator" style="margin-top: 10px; text-align: center; font-weight: bold; color: var(--primary);"></div>
                </div>
                
                <div class="info-panel">
                    <div class="info-title">📊 Peringkat Pemain</div>
                    <div class="player-stats" id="playerStats"></div>
                </div>
            </div>
            
            <div class="dice-area">
                <div class="dice-container">
                    <div class="dice" id="dice1">🎲</div>
                    <div class="dice" id="dice2">🎲</div>
                </div>
                <button class="btn btn-success" id="rollBtn" onclick="rollDice()" style="font-size: 1.3rem;">
                    🎲 LEMPAR DADU
                </button>
                <p id="waitingTurnText" style="margin-top: 10px; font-style: italic; display: none;">
                    Menunggu giliran pemain lain...
                </p>
            </div>
            
            <div style="text-align: center; margin-top: 20px;">
                <button class="btn btn-danger" onclick="location.reload()">🚪 Keluar dari Permainan</button>
            </div>
        </div>
    </div>
    
    <!-- Question Modal -->
    <div class="modal" id="questionModal">
        <div class="modal-content">
            <div class="modal-header">
                <div class="modal-icon" id="modalIcon">❓</div>
                <h3 class="modal-title" id="modalTitle">Pertanyaan</h3>
            </div>
            <div class="question-text" id="questionText"></div>
            <div class="options" id="options"></div>
            <div id="resultArea"></div>
            <button class="btn btn-primary" id="continueBtn" onclick="closeModal()" style="width: 100%; margin-top: 20px; display: none;">
                Lanjutkan ➡️
            </button>
        </div>
    </div>
    
    <!-- Challenge Modal -->
    <div class="modal" id="challengeModal">
        <div class="modal-content">
            <div class="modal-header">
                <div class="modal-icon">💪</div>
                <h3 class="modal-title">Tantangan!</h3>
            </div>
            <div class="challenge-box">
                <div class="challenge-text" id="challengeText"></div>
            </div>
            <button class="btn btn-success" onclick="completeChallenge()" style="width: 100%;">
                ✅ Saya Telah Menyelesaikan Tantangan
            </button>
            <button class="btn btn-danger" onclick="skipChallenge()" style="width: 100%; margin-top: 10px;">
                ⏭️ Lewati (Denda -10 Poin)
            </button>
        </div>
    </div>
    
    <!-- Winner Screen -->
    <div class="winner-screen" id="winnerScreen">
        <div class="trophy">🏆</div>
        <h2 class="winner-title">Selamat!</h2>
        <p class="winner-name" id="winnerName"></p>
        <p style="color: white; font-size: 1.2rem; margin-bottom: 30px;">
            Telah menyelesaikan perjalanan hidup dalam nilai-nilai Kristiani!
        </p>
        <button class="btn btn-success" onclick="location.reload()" style="font-size: 1.2rem;">
            🔄 Main Lagi
        </button>
    </div>

    <script>
        // ==================== GAME DATA ====================
        const colors = [
            '#ef4444', '#f97316', '#f59e0b', '#84cc16', '#10b981',
            '#06b6d4', '#3b82f6', '#6366f1', '#8b5cf6', '#d946ef',
            '#f43f5e', '#78716c', '#14b8a6', '#0ea5e9', '#a855f7'
        ];
        
        const boardData = [
            { type: 'start', icon: '⛪', title: 'Mulai' },
            { type: 'question', icon: '📖', title: 'Firman Tuhan' },
            { type: 'question', icon: '❤️', title: 'Kasih' },
            { type: 'challenge', icon: '🙏', title: 'Doa' },
            { type: 'blessing', icon: '✨', title: 'Berkat' },
            { type: 'question', icon: '🤝', title: 'Persaudaraan' },
            { type: 'trap', icon: '⚠️', title: 'Godaan' },
            { type: 'question', icon: '🕊️', title: 'Damai' },
            { type: 'challenge', icon: '💪', title: 'Iman' },
            { type: 'question', icon: '🌟', title: 'Harapan' },
            { type: 'blessing', icon: '🎁', title: 'Anugerah' },
            { type: 'question', icon: '😇', title: 'Kesucian' },
            { type: 'challenge', icon: '🔥', title: 'Semangat' },
            { type: 'trap', icon: '🌑', title: 'Dosa' },
            { type: 'question', icon: '📿', title: 'Ketaatan' },
            { type: 'question', icon: '🌈', title: 'Janji' },
            { type: 'blessing', icon: '💝', title: 'Kasih Karunia' },
            { type: 'challenge', icon: '🎭', title: 'Kesaksian' },
            { type: 'question', icon: '⚖️', title: 'Keadilan' },
            { type: 'trap', icon: '💸', title: 'Keserakahan' },
            { type: 'question', icon: '🏠', title: 'Keluarga' },
            { type: 'challenge', icon: '🎤', title: 'Pujian' },
            { type: 'blessing', icon: '🌻', title: 'Pertumbuhan' },
            { type: 'question', icon: '📚', title: 'Pengetahuan' },
            { type: 'challenge', icon: '🌍', title: 'Misi' },
            { type: 'trap', icon: '👿', title: 'Iblis' },
            { type: 'question', icon: '💒', title: 'Gereja' },
            { type: 'blessing', icon: '👑', title: 'Kemenangan' },
            { type: 'question', icon: '⏰', title: 'Kesabaran' },
            { type: 'challenge', icon: '📢', title: 'Injil' },
            { type: 'finish', icon: '🏁', title: 'Finish' }
        ];
        
        const questions = [
            {
                question: "Apa yang dimaksud dengan 'Kasihilah Tuhan Allahmu dengan segenap hatimu'?",
                options: ["Mencintai Tuhan sepenuh hati, jiwa, dan akal budi", "Hanya pergi ke gereja setiap minggu", "Membaca Alkitab sesekali", "Berdoa hanya saat ada masalah"],
                correct: 0
            },
            {
                question: "Mengapa kita harus mengasihi sesama kita?",
                options: ["Karena Tuhan pertama kali mengasihi kita", "Agar dipuji oleh orang lain", "Supaya mendapat hadiah", "Karena diperintahkan guru"],
                correct: 0
            },
            {
                question: "Apa bukti nyata kasih kepada sesama menurut 1 Yohanes 3:18?",
                options: ["Mengasihi dengan perkataan dan perbuatan", "Hanya mengatakan 'I love you'", "Memberi hadiah mahal", "Mengunggah foto di media sosial"],
                correct: 0
            },
            {
                question: "Bagaimana sikap Kristen yang benar saat ditimpa masalah?",
                options: ["Berdoa dan tetap percaya pada Tuhan", "Menyalahkan Tuhan", "Menyerah dan putus asa", "Membalas dendam"],
                correct: 0
            },
            {
                question: "Apa makna 'jalan yang sempit' dalam Matthew 7:14?",
                options: ["Hidup menurut kehendak Tuhan yang memerlukan pengorbanan", "Jalan fisik yang kecil", "Hidup dengan cara yang mudah", "Menghindari semua kesulitan"],
                correct: 0
            },
            {
                question: "Mengapa pengampunan penting dalam hidup Kristen?",
                options: ["Karena kita juga diampuni oleh Tuhan", "Agar terlihat baik di depan orang", "Supaya lawan merasa bersalah", "Karena diwajibkan oleh aturan sekolah"],
                correct: 0
            },
            {
                question: "Apa yang harus dilakukan ketika temanmu berbuat dosa?",
                options: ["Menegur dengan kasih untuk membangun", "Menceritakannya ke semua orang", "Meninggalkannya begitu saja", "Ikut-ikutan berbuat dosa"],
                correct: 0
            },
            {
                question: "Apa tujuan hidup orang Kristen menurut Westminster Catechism?",
                options: ["Glorifikasi Allah dan menikmati Dia selamanya", "Menjadi kaya dan terkenal", "Mengejar kesenangan duniawi", "Menjadi yang terbaik di sekolah"],
                correct: 0
            },
            {
                question: "Bagaimana cara menghormati orang tua menurut Efesus 6:1-3?",
                options: ["Taat dan menghormati mereka dalam Tuhan", "Hanya memberi uang pensiun", "Mengikuti semua perintah tanpa pertimbangan", "Menghindari mereka saat dewasa"],
                correct: 0
            },
            {
                question: "Apa arti 'mengambil salib dan mengikut Kristus'?",
                options: ["Rela berkorban dan menyangkal diri demi Kristus", "Membawa salib kayu setiap hari", "Tinggal di gereja terus-menerus", "Menghindari semua kesenangan"],
                correct: 0
            }
        ];
        
        const challenges = [
            "Ucapkan doa syukur untuk 3 hal dalam hidupmu hari ini",
            "Nyanyikan satu ayat dari lagu rohani favoritmu",
            "Ceritakan satu ayat Alkitab yang paling berarti bagimu",
            "Sebutkan 5 nama yang perlu didoakan hari ini",
            "Ucapkan permintaan maaf kepada seseorang yang pernah kamu sakiti",
            "Bagikan firman Tuhan kepada satu orang via chat",
            "Lakukan satu perbuatan baik tanpa diketahui orang lain hari ini",
            "Tulis 3 hal yang kamu syukuri dalam notes ponselmu",
            "Berikan senyum dan sapa kepada 3 orang yang belum kamu kenal",
            "Baca satu pasal dari Mazmur dengan suara lantang",
            "Doakan musuhmu atau orang yang sulit kamu kasihi",
            "Berikan persembahan dengan sukacita (bukan hanya uang)",
            "Hubungi orang tua/saudara dan katakan 'Aku sayang kamu'",
            "Matikan ponselmu selama 30 menit untuk bersekutu dengan Tuhan",
            "Tuliskan impianmu untuk kemuliaan Tuhan 5 tahun ke depan",
            "Memoralkan satu ayat firman Tuhan dalam 2 menit",
            "Ceritakan pengalaman imanmu kepada teman kelasmu",
            "Lakukan satu tindakan pelestarian lingkungan sebagai wujud syukur",
            "Buat daftar 10 berkat Tuhan dalam hidupmu minggu ini",
            "Berjanji untuk tidak mengeluh selama 24 jam ke depan"
        ];
        
        // ==================== STATE VARIABLES ====================
        let peer = null;
        let connections = [];
        let isHost = false;
        let roomCode = '';
        let myPlayerId = 0;
        let players = [];
        let currentPlayerIndex = 0;
        let gameStarted = false;
        let localPlayerName = '';
        
        // ==================== PEERJS SETUP ====================
        function generateRoomCode() {
            return Math.random().toString(36).substring(2, 10).toUpperCase();
        }
        
        function updateConnectionStatus(status, type) {
            const statusDiv = document.getElementById('connectionStatus');
            statusDiv.innerHTML = `<span class="connection-status status-${type}">${status}</span>`;
        }
        
        function showCreateRoom() {
            document.getElementById('mainMenu').style.display = 'none';
            document.getElementById('createRoomScreen').style.display = 'block';
            
            roomCode = generateRoomCode();
            document.getElementById('hostCode').textContent = roomCode;
            
            isHost = true;
            myPlayerId = 0;
            
            // Initialize PeerJS as host
            peer = new Peer(roomCode, {
                host: '0.peerjs.com',
                secure: true,
                port: 443,
                config: {
                    'iceServers': [
                        { urls: 'stun:stun.l.google.com:19302' },
                        { urls: 'stun:stun1.l.google.com:19302' }
                    ]
                }
            });
            
            peer.on('open', (id) => {
                console.log('Host peer opened with ID:', id);
                updateConnectionStatus('🟢 Host Terhubung - Menunggu Pemain', 'host');
                
                // Add host as first player
                localPlayerName = prompt('Masukkan nama kamu (Host):') || 'Host';
                players.push({
                    id: 0,
                    name: localPlayerName,
                    color: colors[0],
                    position: 0,
                    score: 0,
                    finished: false,
                    isHost: true
                });
                updatePlayersList();
            });
            
            peer.on('connection', (conn) => {
                console.log('New connection:', conn.peer);
                connections.push(conn);
                
                const playerId = players.length;
                
                conn.on('open', () => {
                    conn.send({
                        type: 'assignId',
                        playerId: playerId,
                        color: colors[playerId % colors.length]
                    });
                });
                
                conn.on('data', (data) => {
                    handleData(data, conn);
                });
                
                conn.on('close', () => {
                    console.log('Connection closed:', conn.peer);
                    connections = connections.filter(c => c !== conn);
                    // Remove player
                    players = players.filter(p => p.conn !== conn);
                    updatePlayersList();
                    broadcast({ type: 'updatePlayers', players: players });
                });
            });
            
            peer.on('error', (err) => {
                console.error('Peer error:', err);
                alert('Error: ' + err.message);
            });
        }
        
        function showJoinRoom() {
            document.getElementById('mainMenu').style.display = 'none';
            document.getElementById('joinRoomScreen').style.display = 'block';
        }
        
        function joinRoom() {
            const code = document.getElementById('joinCodeInput').value.trim().toUpperCase();
            localPlayerName = document.getElementById('joinPlayerName').value.trim();
            
            if (!code || !localPlayerName) {
                alert('Masukkan kode room dan nama!');
                return;
            }
            
            roomCode = code;
            isHost = false;
            
            updateConnectionStatus('🟡 Menghubungkan ke room...', 'connecting');
            
            // Generate unique ID for this peer
            const peerId = code + '_player_' + Math.random().toString(36).substr(2, 5);
            
            peer = new Peer(peerId, {
                host: '0.peerjs.com',
                secure: true,
                port: 443,
                config: {
                    'iceServers': [
                        { urls: 'stun:stun.l.google.com:19302' },
                        { urls: 'stun:stun1.l.google.com:19302' }
                    ]
                }
            });
            
            peer.on('open', () => {
                console.log('Joiner peer opened');
                const conn = peer.connect(code, {
                    reliable: true,
                    serialization: 'json'
                });
                
                connections.push(conn);
                
                conn.on('open', () => {
                    console.log('Connected to host');
                    updateConnectionStatus('🟢 Terhubung ke Room', 'connected');
                    conn.send({
                        type: 'join',
                        name: localPlayerName
                    });
                });
                
                conn.on('data', (data) => {
                    handleData(data, conn);
                });
                
                conn.on('close', () => {
                    alert('Koneksi ke host terputus!');
                    location.reload();
                });
                
                conn.on('error', (err) => {
                    console.error('Connection error:', err);
                    alert('Gagal terhubung. Pastikan kode benar dan host online.');
                });
            });
            
            peer.on('error', (err) => {
                console.error('Peer error:', err);
            });
        }
        
        function handleData(data, conn) {
            console.log('Received:', data);
            
            switch(data.type) {
                case 'assignId':
                    myPlayerId = data.playerId;
                    updateConnectionStatus(`🟢 Terhubung sebagai Pemain ${myPlayerId + 1}`, 'client');
                    alert(`Berhasil bergabung! Kamu adalah Pemain ${myPlayerId + 1}`);
                    document.getElementById('joinRoomScreen').innerHTML = `
                        <h2 class="menu-title">✅ Berhasil Bergabung!</h2>
                        <p>Menunggu host memulai permainan...</p>
                        <div class="connection-status status-connected">Terhubung</div>
                    `;
                    break;
                    
                case 'join':
                    if (isHost) {
                        const newPlayer = {
                            id: players.length,
                            name: data.name,
                            color: colors[players.length % colors.length],
                            position: 0,
                            score: 0,
                            finished: false,
                            isHost: false,
                            conn: conn
                        };
                        players.push(newPlayer);
                        updatePlayersList();
                        broadcast({ type: 'updatePlayers', players: players });
                    }
                    break;
                    
                case 'updatePlayers':
                    if (!isHost) {
                        players = data.players;
                    }
                    break;
                    
                case 'gameStart':
                    players = data.players;
                    startGameUI();
                    break;
                    
                case 'rollDice':
                    if (!isHost) {
                        animateDice(data.dice1, data.dice2);
                        setTimeout(() => {
                            movePlayer(data.playerId, data.steps);
                        }, 1000);
                    }
                    break;
                    
                case 'playerMove':
                    updatePlayerPosition(data.playerId, data.position);
                    break;
                    
                case 'updateScores':
                    players = data.players;
                    currentPlayerIndex = data.currentPlayerIndex;
                    updateGameInfo();
                    break;
                    
                case 'nextTurn':
                    currentPlayerIndex = data.currentPlayerIndex;
                    updateGameInfo();
                    break;
                    
                case 'gameOver':
                    showWinnerScreen(data.winner);
                    break;
            }
        }
        
        function broadcast(data) {
            connections.forEach(conn => {
                if (conn.open) {
                    conn.send(data);
                }
            });
        }
        
        function updatePlayersList() {
            const listDiv = document.getElementById('playersListContent');
            listDiv.innerHTML = players.map((p, i) => `
                <div class="player-item">
                    <div class="player-color" style="background: ${p.color}"></div>
                    <div>
                        <strong>${p.name}</strong> ${p.isHost ? '(Host)' : ''}
                        <br><small>Pemain ${i + 1}</small>
                    </div>
                </div>
            `).join('');
            
            const startBtn = document.getElementById('startGameBtn');
            if (players.length >= 1) {
                startBtn.disabled = false;
                startBtn.textContent = `🎲 Mulai Permainan (${players.length} Pemain)`;
            }
        }
        
        function hostStartGame() {
            if (players.length < 1) {
                alert('Butuh minimal 1 pemain!');
                return;
            }
            
            gameStarted = true;
            broadcast({ type: 'gameStart', players: players });
            startGameUI();
        }
        
        function startLocalGame() {
            isHost = true;
            myPlayerId = 0;
            gameStarted = true;
            
            const name = prompt('Masukkan nama:') || 'Player 1';
            players.push({
                id: 0,
                name: name,
                color: colors[0],
                position: 0,
                score: 0,
                finished: false,
                isHost: true
            });
            
            // Add AI or second player locally
            const mode = confirm('Tambah pemain kedua (lokal)? \nOK = Ya, Cancel = Main sendiri');
            if (mode) {
                const name2 = prompt('Nama Pemain 2:') || 'Player 2';
                players.push({
                    id: 1,
                    name: name2,
                    color: colors[1],
                    position: 0,
                    score: 0,
                    finished: false,
                    isHost: false
                });
            }
            
            document.getElementById('mainMenu').style.display = 'none';
            startGameUI();
        }
        
        // ==================== GAME UI ====================
        function startGameUI() {
            document.getElementById('createRoomScreen').style.display = 'none';
            document.getElementById('joinRoomScreen').style.display = 'none';
            document.getElementById('gameBoard').style.display = 'block';
            
            renderBoard();
            updateGameInfo();
        }
        
        function renderBoard() {
            const container = document.getElementById('boardContainer');
            container.innerHTML = '';
            
            boardData.forEach((cell, index) => {
                const cellDiv = document.createElement('div');
                cellDiv.className = `cell cell-${cell.type}`;
                cellDiv.id = `cell-${index}`;
                cellDiv.innerHTML = `
                    <span class="cell-number">${index}</span>
                    <span class="cell-icon">${cell.icon}</span>
                    <span class="cell-title">${cell.title}</span>
                `;
                container.appendChild(cellDiv);
            });
            
            // Place tokens
            players.forEach((player, index) => {
                const token = document.createElement('div');
                token.className = 'player-token';
                token.id = `token-${index}`;
                token.style.background = player.color;
                token.textContent = index + 1;
                document.getElementById('cell-0').appendChild(token);
            });
        }
        
        function updateGameInfo() {
            const current = players[currentPlayerIndex];
            const isMyTurn = currentPlayerIndex === myPlayerId;
            
            document.getElementById('currentPlayer').innerHTML = `
                <div class="player-avatar" style="background: ${current.color}">
                    ${currentPlayerIndex + 1}
                </div>
                <div>
                    <div style="font-weight: 700; font-size: 1.2rem; color: var(--dark);">${current.name}</div>
                    <div style="color: #64748b;">Posisi: ${current.position} | Poin: ${current.score}</div>
                </div>
            `;
            
            const turnIndicator = document.getElementById('yourTurnIndicator');
            const rollBtn = document.getElementById('rollBtn');
            const waitingText = document.getElementById('waitingTurnText');
            
            if (isMyTurn) {
                turnIndicator.textContent = '🎮 Giliran Kamu!';
                rollBtn.disabled = false;
                rollBtn.style.display = 'block';
                waitingText.style.display = 'none';
            } else {
                turnIndicator.textContent = `⏳ Giliran ${current.name}...`;
                rollBtn.disabled = true;
                rollBtn.style.display = 'none';
                waitingText.style.display = 'block';
            }
            
            document.getElementById('playerStats').innerHTML = players.map((p, i) => `
                <div class="stat-card ${i === currentPlayerIndex ? 'pulse' : ''}" style="${i === currentPlayerIndex ? 'border: 2px solid ' + p.color : ''}">
                    <div class="stat-color" style="background: ${p.color}"></div>
                    <div class="stat-info">
                        <div class="stat-name">${p.name} ${p.finished ? '✅' : ''} ${i === myPlayerId ? '(Kamu)' : ''}</div>
                        <div class="stat-score">Pos: ${p.position} | Poin: ${p.score}</div>
                    </div>
                </div>
            `).join('');
        }
        
        // ==================== GAME LOGIC ====================
        function rollDice() {
            if (currentPlayerIndex !== myPlayerId && !isHost && connections.length > 0) {
                return; // Bukan giliran kita
            }
            
            const btn = document.getElementById('rollBtn');
            btn.disabled = true;
            
            const dice1 = Math.floor(Math.random() * 6) + 1;
            const dice2 = Math.floor(Math.random() * 6) + 1;
            const total = dice1 + dice2;
            
            animateDice(dice1, dice2);
            
            // Broadcast to all players
            if (isHost || connections.length > 0) {
                broadcast({
                    type: 'rollDice',
                    dice1: dice1,
                    dice2: dice2,
                    steps: total,
                    playerId: currentPlayerIndex
                });
            }
            
            setTimeout(() => {
                movePlayer(currentPlayerIndex, total);
            }, 1000);
        }
        
        function animateDice(val1, val2) {
            const dice1El = document.getElementById('dice1');
            const dice2El = document.getElementById('dice2');
            
            dice1El.classList.add('rolling');
            dice2El.classList.add('rolling');
            
            let rolls = 0;
            const interval = setInterval(() => {
                dice1El.textContent = Math.floor(Math.random() * 6) + 1;
                dice2El.textContent = Math.floor(Math.random() * 6) + 1;
                rolls++;
                
                if (rolls >= 10) {
                    clearInterval(interval);
                    dice1El.classList.remove('rolling');
                    dice2El.classList.remove('rolling');
                    dice1El.textContent = val1;
                    dice2El.textContent = val2;
                }
            }, 100);
        }
        
        function movePlayer(playerIndex, steps) {
            const player = players[playerIndex];
            const oldPos = player.position;
            let newPos = oldPos + steps;
            
            if (newPos >= boardData.length - 1) {
                newPos = boardData.length - 1;
                player.finished = true;
            }
            
            // Animate
            const token = document.getElementById(`token-${playerIndex}`);
            let currentStep = oldPos;
            
            const moveInterval = setInterval(() => {
                if (currentStep >= newPos) {
                    clearInterval(moveInterval);
                    player.position = newPos;
                    
                    const newCell = document.getElementById(`cell-${newPos}`);
                    newCell.appendChild(token);
                    
                    handleCellLanding(playerIndex, newPos);
                    return;
                }
                
                currentStep++;
                const nextCell = document.getElementById(`cell-${currentStep}`);
                nextCell.appendChild(token);
            }, 300);
            
            // Broadcast position update
            broadcast({
                type: 'playerMove',
                playerId: playerIndex,
                position: newPos
            });
        }
        
        function updatePlayerPosition(playerId, position) {
            const player = players[playerId];
            player.position = position;
            
            const token = document.getElementById(`token-${playerId}`);
            const cell = document.getElementById(`cell-${position}`);
            cell.appendChild(token);
        }
        
        function handleCellLanding(playerIndex, position) {
            const cell = boardData[position];
            const player = players[playerIndex];
            
            switch(cell.type) {
                case 'start':
                    player.score += 10;
                    showResult('Selamat datang! +10 poin', true);
                    break;
                    
                case 'finish':
                    player.score += 100;
                    endGame();
                    break;
                    
                case 'question':
                    if (playerIndex === myPlayerId || (isHost && connections.length === 0)) {
                        showQuestion();
                    } else {
                        setTimeout(nextTurn, 1000);
                    }
                    break;
                    
                case 'challenge':
                    if (playerIndex === myPlayerId || (isHost && connections.length === 0)) {
                        showChallenge();
                    } else {
                        setTimeout(nextTurn, 1000);
                    }
                    break;
                    
                case 'blessing':
                    const blessingPoints = Math.floor(Math.random() * 20) + 10;
                    player.score += blessingPoints;
                    showResult(`🎉 Berkah Tuhan! +${blessingPoints} poin`, true);
                    break;
                    
                case 'trap':
                    const trapPenalty = Math.floor(Math.random() * 15) + 5;
                    player.score -= trapPenalty;
                    if (player.position > 2) {
                        player.position -= 2;
                        setTimeout(() => {
                            const token = document.getElementById(`token-${playerIndex}`);
                            const newCell = document.getElementById(`cell-${player.position}`);
                            newCell.appendChild(token);
                        }, 1000);
                    }
                    showResult(`⚠️ ${cell.title}! -${trapPenalty} poin dan mundur 2 langkah`, false);
                    break;
            }
            
            broadcast({
                type: 'updateScores',
                players: players,
                currentPlayerIndex: currentPlayerIndex
            });
            
            updateGameInfo();
        }
        
        function showQuestion() {
            const modal = document.getElementById('questionModal');
            const q = questions[Math.floor(Math.random() * questions.length)];
            
            document.getElementById('questionText').textContent = q.question;
            const optionsDiv = document.getElementById('options');
            optionsDiv.innerHTML = '';
            
            q.options.forEach((opt, index) => {
                const btn = document.createElement('div');
                btn.className = 'option';
                btn.textContent = opt;
                btn.onclick = () => checkAnswer(index, q.correct, q.options);
                optionsDiv.appendChild(btn);
            });
            
            document.getElementById('resultArea').innerHTML = '';
            document.getElementById('continueBtn').style.display = 'none';
            
            modal.classList.add('active');
        }
        
        function checkAnswer(selected, correct, options) {
            const optionDivs = document.querySelectorAll('.option');
            optionDivs.forEach((div, index) => {
                div.onclick = null;
                if (index === correct) div.classList.add('correct');
                else if (index === selected && index !== correct) div.classList.add('wrong');
            });
            
            const player = players[currentPlayerIndex];
            if (selected === correct) {
                player.score += 20;
                document.getElementById('resultArea').innerHTML = `<div class="result-message result-success">✅ Benar! +20 poin</div>`;
            } else {
                player.score -= 5;
                document.getElementById('resultArea').innerHTML = `<div class="result-message result-fail">❌ Salah! Jawaban: ${options[correct]}. -5 poin</div>`;
            }
            
            document.getElementById('continueBtn').style.display = 'block';
            updateGameInfo();
        }
        
        function showChallenge() {
            const modal = document.getElementById('challengeModal');
            const challenge = challenges[Math.floor(Math.random() * challenges.length)];
            document.getElementById('challengeText').textContent = challenge;
            modal.classList.add('active');
        }
        
        function completeChallenge() {
            const player = players[currentPlayerIndex];
            player.score += 15;
            document.getElementById('challengeModal').classList.remove('active');
            showResult('💪 Tantangan selesai! +15 poin', true);
            updateGameInfo();
        }
        
        function skipChallenge() {
            const player = players[currentPlayerIndex];
            player.score -= 10;
            document.getElementById('challengeModal').classList.remove('active');
            showResult('⏭️ Tantangan dilewati. -10 poin', false);
            updateGameInfo();
        }
        
        function showResult(message, isSuccess) {
            const modal = document.getElementById('questionModal');
            document.getElementById('modalIcon').textContent = isSuccess ? '🎉' : '😅';
            document.getElementById('modalTitle').textContent = isSuccess ? 'Hebat!' : 'Terus Berusaha!';
            document.getElementById('questionText').innerHTML = `<div style="text-align: center; font-size: 1.3rem; padding: 20px;">${message}</div>`;
            document.getElementById('options').innerHTML = '';
            document.getElementById('resultArea').innerHTML = '';
            document.getElementById('continueBtn').style.display = 'block';
            modal.classList.add('active');
        }
        
        function closeModal() {
            document.getElementById('questionModal').classList.remove('active');
            nextTurn();
        }
        
        function nextTurn() {
            currentPlayerIndex = (currentPlayerIndex + 1) % players.length;
            
            // Skip finished players
            let loopCount = 0;
            while (players[currentPlayerIndex].finished && loopCount < players.length) {
                currentPlayerIndex = (currentPlayerIndex + 1) % players.length;
                loopCount++;
            }
            
            broadcast({
                type: 'nextTurn',
                currentPlayerIndex: currentPlayerIndex
            });
            
            updateGameInfo();
        }
        
        function endGame() {
            const sorted = [...players].sort((a, b) => b.score - a.score);
            const winner = sorted[0];
            
            broadcast({
                type: 'gameOver',
                winner: winner
            });
            
            showWinnerScreen(winner);
        }
        
        function showWinnerScreen(winner) {
            document.getElementById('winnerName').textContent = winner.name;
            document.getElementById('winnerScreen').classList.add('active');
        }
    </script>
</body>
</html>
