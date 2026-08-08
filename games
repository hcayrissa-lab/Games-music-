<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rhythm Music Game</title>
  <style>
    body {
      background-color: #1a1a1a;
      color: #fff;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }

    h1 {
      margin-bottom: 10px;
    }

    .score-board {
      font-size: 24px;
      margin-bottom: 15px;
    }

    /* Track / Area Permainan */
    .game-container {
      position: relative;
      width: 320px;
      height: 500px;
      background-color: #2b2b2b;
      border: 3px solid #00fff2;
      border-radius: 8px;
      overflow: hidden;
      display: flex;
    }

    /* Jalur Tombol (Lanes) */
    .lane {
      flex: 1;
      border-right: 1px dashed #444;
      position: relative;
    }

    .lane:last-child {
      border-right: none;
    }

    /* Garis Target Tekan */
    .target-line {
      position: absolute;
      bottom: 40px;
      width: 100%;
      height: 50px;
      background-color: rgba(255, 255, 255, 0.2);
      border-top: 2px solid #ff0055;
      border-bottom: 2px solid #ff0055;
      pointer-events: none;
    }

    /* Tile Musik yang Jatuh */
    .tile {
      position: absolute;
      width: 100%;
      height: 60px;
      background: linear-gradient(180deg, #00fff2, #0088ff);
      border-radius: 4px;
    }

    /* Label Tombol Kontrol */
    .controls {
      display: flex;
      width: 320px;
      margin-top: 10px;
    }

    .key-label {
      flex: 1;
      text-align: center;
      background-color: #333;
      padding: 10px 0;
      border: 1px solid #555;
      font-weight: bold;
      border-radius: 4px;
    }
  </style>
</head>
<body>

  <h1>🎵 Rhythm Tap Game</h1>
  <div class="score-board">Skor: <span id="score">0</span></div>

  <div class="game-container">
    <div class="lane" id="lane-0"></div>
    <div class="lane" id="lane-1"></div>
    <div class="lane" id="lane-2"></div>
    <div class="lane" id="lane-3"></div>
    <div class="target-line"></div>
  </div>

  <div class="controls">
    <div class="key-label">A</div>
    <div class="key-label">S</div>
    <div class="key-label">D</div>
    <div class="key-label">F</div>
  </div>

  <script>
    const keys = ['a', 's', 'd', 'f'];
    const lanes = [
      document.getElementById('lane-0'),
      document.getElementById('lane-1'),
      document.getElementById('lane-2'),
      document.getElementById('lane-3')
    ];
    const scoreDisplay = document.getElementById('score');

    let score = 0;
    let activeTiles = []; // Menyimpan data tile yang sedang aktif
    const speed = 4; // Kecepatan jatuhnya tile (pixel per frame)

    // Fungsi untuk membuat nada menggunakan Web Audio API
    function playNote(frequency) {
      const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      const oscillator = audioCtx.createOscillator();
      const gainNode = audioCtx.createGain();

      oscillator.type = 'sine';
      oscillator.frequency.value = frequency;

      gainNode.gain.setValueAtTime(0.1, audioCtx.currentTime);
      gainNode.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.3);

      oscillator.connect(gainNode);
      gainNode.connect(audioCtx.destination);

      oscillator.start();
      oscillator.stop(audioCtx.currentTime + 0.3);
    }

    // Frekuensi nada untuk tiap jalur (A, S, D, F -> Do, Re, Mi, Fa)
    const frequencies = [261.63, 293.66, 329.63, 349.23];

    // Generasi Tile secara berkala
    function spawnTile() {
      const laneIndex = Math.floor(Math.random() * 4);
      const tileElem = document.createElement('div');
      tileElem.classList.add('tile');
      tileElem.style.top = '-60px';

      lanes[laneIndex].appendChild(tileElem);

      activeTiles.push({
        element: tileElem,
        lane: laneIndex,
        top: -60
      });
    }

    // Loop utama untuk menggerakkan tile
    function gameLoop() {
      for (let i = activeTiles.length - 1; i >= 0; i--) {
        let tile = activeTiles[i];
        tile.top += speed;
        tile.element.style.top = tile.top + 'px';

        // Jika tile melewati batas bawah (meleset)
        if (tile.top > 500) {
          tile.element.remove();
          activeTiles.splice(i, 1);
        }
      }
      requestAnimationFrame(gameLoop);
    }

    // Deteksi Tekanan Tombol Keyboard
    window.addEventListener('keydown', (e) => {
      const keyIndex = keys.indexOf(e.key.toLowerCase());

      if (keyIndex !== -1) {
        // Cari tile di jalur tersebut yang paling dekat dengan garis target (posisi y: 410-470)
        const hitIndex = activeTiles.findIndex(tile => 
          tile.lane === keyIndex && tile.top >= 370 && tile.top <= 470
        );

        if (hitIndex !== -1) {
          // Berhasil menekan nada
          playNote(frequencies[keyIndex]);
          score += 10;
          scoreDisplay.textContent = score;

          // Hapus tile yang berhasil ditekan
          activeTiles[hitIndex].element.remove();
          activeTiles.splice(hitIndex, 1);
        }
      }
    });

    // Jalankan Game
    setInterval(spawnTile, 1000); // Tile baru setiap 1 detik
    requestAnimationFrame(gameLoop);
  </script>
</body>
</html>
