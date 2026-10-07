<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Neon Overdrive - Retro Synthwave Racer</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Press+Start+2P&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Orbitron', sans-serif;
            background-color: #0d0221;
            color: #ffffff;
            overflow: hidden;
            touch-action: none;
            user-select: none;
        }
        .retro-font {
            font-family: 'Press Start 2P', cursive;
        }
        .neon-text-cyan {
            text-shadow: 0 0 5px #00f0ff, 0 0 10px #00f0ff, 0 0 20px #00f0ff;
        }
        .neon-text-magenta {
            text-shadow: 0 0 5px #ff007f, 0 0 10px #ff007f, 0 0 20px #ff007f;
        }
        .neon-border {
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.4), inset 0 0 15px rgba(0, 240, 255, 0.2);
        }
        .neon-button {
            transition: all 0.2s ease;
            box-shadow: 0 0 10px #ff007f;
        }
        .neon-button:hover {
            box-shadow: 0 0 20px #ff007f, 0 0 30px #ff007f;
            transform: translateY(-2px);
        }
        .neon-button:active {
            transform: translateY(1px);
        }
        canvas {
            display: block;
            width: 100%;
            height: 100%;
        }
    </style>
</head>
<body class="w-screen h-screen flex flex-col justify-center items-center bg-black relative">

    <!-- Main Game Screen Container -->
    <div id="gameContainer" class="relative w-full h-full max-w-5xl max-h-[900px] flex flex-col justify-between items-center overflow-hidden border-0 md:border-2 border-cyan-500 rounded-none md:rounded-2xl neon-border">
        
        <!-- Canvas Game Layer -->
        <canvas id="gameCanvas" class="absolute inset-0 z-0"></canvas>

        <!-- Dynamic HUD Overlay -->
        <div id="hudOverlay" class="relative z-10 w-full p-4 flex justify-between items-start pointer-events-none hidden">
            <!-- Left HUD: Speed & Gear -->
            <div class="bg-black/60 backdrop-blur-md p-3 rounded-xl border border-cyan-500/50 flex flex-col items-center min-w-[130px]">
                <span class="text-xs text-cyan-400 font-semibold tracking-widest">KECEPATAN</span>
                <div class="flex items-baseline space-x-1">
                    <span id="speedValue" class="text-3xl font-black text-white neon-text-cyan">0</span>
                    <span class="text-xs text-gray-400">KM/H</span>
                </div>
                <!-- Speedometer Arc Progress -->
                <div class="w-full bg-gray-800 h-2 rounded-full mt-2 overflow-hidden border border-cyan-900">
                    <div id="speedBar" class="bg-gradient-to-r from-cyan-500 to-magenta-500 h-full w-0 transition-all duration-100"></div>
                </div>
            </div>

            <!-- Center HUD: Distance & High Score -->
            <div class="bg-black/60 backdrop-blur-md px-6 py-2 rounded-xl border border-magenta-500/50 flex flex-col items-center">
                <span class="text-[10px] text-magenta-400 tracking-widest">JARAK TEMPUH</span>
                <span id="scoreValue" class="text-2xl font-black text-yellow-400 tracking-wider">0000 m</span>
                <div class="text-[10px] text-gray-400 mt-0.5">REKOR: <span id="highScoreHUD" class="text-white">0 m</span></div>
            </div>

            <!-- Right HUD: Nitro Gauge -->
            <div class="bg-black/60 backdrop-blur-md p-3 rounded-xl border border-cyan-500/50 flex flex-col items-center min-w-[130px]">
                <span class="text-xs text-yellow-400 font-semibold tracking-widest">NITRO BOOST</span>
                <div class="w-full bg-gray-800 h-4 rounded-full mt-2 p-0.5 border border-yellow-500/50 overflow-hidden">
                    <div id="nitroBar" class="bg-gradient-to-r from-yellow-500 to-red-500 h-full w-full rounded-full transition-all duration-75"></div>
                </div>
                <span id="nitroStatus" class="text-[10px] text-yellow-300 mt-1 font-bold">READY (SPACE)</span>
            </div>
        </div>

        <!-- Touch Controls for Mobile Devices -->
        <div id="touchControls" class="relative z-20 w-full p-4 pb-6 flex justify-between items-end pointer-events-auto md:hidden hidden">
            <!-- Left / Right Steer -->
            <div class="flex space-x-3">
                <button id="btnLeft" class="w-16 h-16 bg-cyan-600/60 active:bg-cyan-400 rounded-full border-2 border-cyan-300 text-white text-2xl font-bold flex items-center justify-center backdrop-blur-sm">◄</button>
                <button id="btnRight" class="w-16 h-16 bg-cyan-600/60 active:bg-cyan-400 rounded-full border-2 border-cyan-300 text-white text-2xl font-bold flex items-center justify-center backdrop-blur-sm">►</button>
            </div>
            <!-- Gas / Brake / Nitro -->
            <div class="flex space-x-3">
                <button id="btnBrake" class="w-14 h-14 bg-red-600/60 active:bg-red-400 rounded-full border-2 border-red-300 text-white text-xs font-bold flex items-center justify-center backdrop-blur-sm">REM</button>
                <button id="btnNitro" class="w-14 h-14 bg-yellow-600/60 active:bg-yellow-400 rounded-full border-2 border-yellow-300 text-black text-xs font-black flex items-center justify-center backdrop-blur-sm">NITRO</button>
                <button id="btnGas" class="w-16 h-16 bg-green-600/60 active:bg-green-400 rounded-full border-2 border-green-300 text-white text-lg font-bold flex items-center justify-center backdrop-blur-sm">GAS</button>
            </div>
        </div>

        <!-- START / MAIN MENU OVERLAY -->
        <div id="startMenu" class="absolute inset-0 z-30 flex flex-col justify-center items-center bg-black/85 backdrop-blur-md p-6 text-center">
            <h1 class="text-4xl md:text-6xl font-black italic text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-magenta-500 to-yellow-400 mb-2 neon-text-magenta tracking-wider">
                NEON OVERDRIVE
            </h1>
            <p class="text-cyan-300 text-xs md:text-sm mb-6 tracking-widest">CYBERPUNK SYNTHWAVE RACER</p>

            <!-- Car Selector -->
            <div class="mb-6 bg-gray-900/80 p-4 rounded-xl border border-cyan-500/40 w-full max-w-sm">
                <p class="text-xs text-gray-300 mb-3 font-semibold">PILIH MOBIL BALAP:</p>
                <div class="flex justify-center space-x-4">
                    <button onclick="selectCar('cyan')" id="carBtnCyan" class="px-4 py-2 rounded-lg border-2 border-cyan-400 bg-cyan-950/80 text-cyan-300 text-xs font-bold transition">NEON CYAN</button>
                    <button onclick="selectCar('magenta')" id="carBtnMagenta" class="px-4 py-2 rounded-lg border-2 border-gray-600 bg-gray-900 text-gray-400 text-xs font-bold transition">MAGENTA RACER</button>
                    <button onclick="selectCar('yellow')" id="carBtnYellow" class="px-4 py-2 rounded-lg border-2 border-gray-600 bg-gray-900 text-gray-400 text-xs font-bold transition">GOLD VIPER</button>
                </div>
            </div>

            <button id="btnStart" class="neon-button px-8 py-4 bg-gradient-to-r from-magenta-600 to-purple-600 text-white rounded-xl text-lg font-black tracking-wider uppercase border border-magenta-300 cursor-pointer">
                MULAI BALAPAN
            </button>

            <!-- Keyboard Instructions -->
            <div class="mt-8 text-xs text-gray-400 space-y-1 hidden md:block bg-black/50 p-3 rounded-lg border border-gray-800">
                <p><span class="text-cyan-400 font-bold">W / ↑</span> : Gas &nbsp;|&nbsp; <span class="text-cyan-400 font-bold">S / ↓</span> : Rem &nbsp;|&nbsp; <span class="text-cyan-400 font-bold">A / D / ← / →</span> : Kemudi</p>
                <p><span class="text-yellow-400 font-bold">SPACEBAR</span> : Nitro Boost &nbsp;|&nbsp; <span class="text-magenta-400 font-bold">P</span> : Pause Game</p>
            </div>
        </div>

        <!-- PAUSE OVERLAY -->
        <div id="pauseMenu" class="absolute inset-0 z-30 flex flex-col justify-center items-center bg-black/80 backdrop-blur-md p-6 text-center hidden">
            <h2 class="text-4xl font-bold text-yellow-400 mb-4 neon-text-cyan">GAME DI-PAUSE</h2>
            <button id="btnResume" class="neon-button px-6 py-3 bg-cyan-600 text-white rounded-xl text-md font-bold border border-cyan-300 mb-3 w-48">
                LANJUTKAN
            </button>
            <button id="btnRestartPause" class="px-6 py-3 bg-gray-800 text-gray-300 rounded-xl text-md font-bold border border-gray-600 w-48 hover:bg-gray-700">
                ULANGI BALAPAN
            </button>
        </div>

        <!-- GAME OVER OVERLAY -->
        <div id="gameOverMenu" class="absolute inset-0 z-30 flex flex-col justify-center items-center bg-black/90 backdrop-blur-md p-6 text-center hidden">
            <h2 class="text-4xl md:text-5xl font-black text-red-500 mb-2 neon-text-magenta">TABRAKAN!</h2>
            <p class="text-gray-300 text-sm mb-6">Mobil Anda hancur dalam kecepatan tinggi.</p>

            <div class="bg-gray-900/90 border border-red-500/50 p-5 rounded-2xl mb-6 w-full max-w-xs space-y-3">
                <div class="flex justify-between items-center text-sm">
                    <span class="text-gray-400">Jarak Tempuh:</span>
                    <span id="finalScore" class="text-xl font-bold text-yellow-400">0 m</span>
                </div>
                <div class="flex justify-between items-center text-sm">
                    <span class="text-gray-400">Rekor Tertinggi:</span>
                    <span id="finalHighScore" class="text-xl font-bold text-cyan-400">0 m</span>
                </div>
            </div>

            <button id="btnRestart" class="neon-button px-8 py-4 bg-gradient-to-r from-red-600 to-magenta-600 text-white rounded-xl text-lg font-black tracking-wider uppercase border border-red-300 cursor-pointer">
                MAIN LAGI
            </button>
        </div>

    </div>

    <script>
        // ==========================================
        // SYNTHETIC AUDIO ENGINE (Web Audio API)
        // ==========================================
        class SoundEngine {
            constructor() {
                this.ctx = null;
                this.engineOsc = null;
                this.engineGain = null;
                this.isMuted = false;
            }

            init() {
                if (!this.ctx) {
                    const AudioContext = window.AudioContext || window.webkitAudioContext;
                    this.ctx = new AudioContext();
                }
                if (this.ctx.state === 'suspended') {
                    this.ctx.resume();
                }
            }

            startEngineSound() {
                if (!this.ctx || this.engineOsc) return;
                try {
                    this.engineOsc = this.ctx.createOscillator();
                    this.engineGain = this.ctx.createGain();

                    this.engineOsc.type = 'sawtooth';
                    this.engineOsc.frequency.setValueAtTime(60, this.ctx.currentTime); // Pitch awal mesin rendah

                    this.engineGain.gain.setValueAtTime(0.05, this.ctx.currentTime); // Volume sedang

                    // Lowpass filter untuk menghaluskan suara mesin
                    const filter = this.ctx.createBiquadFilter();
                    filter.type = 'lowpass';
                    filter.frequency.setValueAtTime(400, this.ctx.currentTime);

                    this.engineOsc.connect(filter);
                    filter.connect(this.engineGain);
                    this.engineGain.connect(this.ctx.destination);

                    this.engineOsc.start();
                } catch(e) { console.log(e); }
            }

            updateEnginePitch(speedRatio) {
                if (!this.engineOsc || !this.ctx) return;
                // Menaikkan frekuensi audio sesuai rasio kecepatan
                const freq = 60 + (speedRatio * 220);
                this.engineOsc.frequency.setTargetAtTime(freq, this.ctx.currentTime, 0.05);
            }

            stopEngineSound() {
                if (this.engineOsc) {
                    try {
                        this.engineOsc.stop();
                        this.engineOsc.disconnect();
                    } catch(e) {}
                    this.engineOsc = null;
                }
            }

            playNitroSound() {
                if (!this.ctx) return;
                try {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(300, this.ctx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(800, this.ctx.currentTime + 0.4);

                    gain.gain.setValueAtTime(0.2, this.ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.4);

                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start();
                    osc.stop(this.ctx.currentTime + 0.4);
                } catch(e) {}
            }

            playCrashSound() {
                if (!this.ctx) return;
                try {
                    // Kebisingan putih (White Noise) untuk efek tabrakan
                    const bufferSize = this.ctx.sampleRate * 0.5;
                    const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
                    const output = buffer.getChannelData(0);
                    for (let i = 0; i < bufferSize; i++) {
                        output[i] = Math.random() * 2 - 1;
                    }

                    const whiteNoise = this.ctx.createBufferSource();
                    whiteNoise.buffer = buffer;

                    const gain = this.ctx.createGain();
                    gain.gain.setValueAtTime(0.4, this.ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.5);

                    whiteNoise.connect(gain);
                    gain.connect(this.ctx.destination);

                    whiteNoise.start();
                } catch(e) {}
            }
        }

        const audio = new SoundEngine();

        // ==========================================
        // GAME SETUP & CONFIGURATION
        // ==========================================
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // State & Variables
        let gameState = 'START'; // START, PLAYING, PAUSED, GAMEOVER
        let selectedCarColor = 'cyan';
        let highScore = localStorage.getItem('neon_racer_highscore') || 0;

        // Visual & Road Variables
        let roadOffset = 0;
        let speed = 0;
