<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Game Tembak-Tembakan</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>Game Tembak-Tembakan Sederhana</h1>
    <div id="status">
      <p>Nyawa Kamu: <span id="hpPlayer">5</span></p>
      <p>Nyawa Musuh: <span id="hpEnemy">5</span></p>
    </div>

    <div id="weapon-choice">
      <h2>Pilih Senjata</h2>
      <button onclick="pilihSenjata('Pistol', 1)">Pistol (Damage: 1)</button>
      <button onclick="pilihSenjata('Shotgun', 2)">Shotgun (Damage: 2)</button>
      <button onclick="pilihSenjata('Sniper', 3)">Sniper (Damage: 3)</button>
    </div>

    <div id="log"></div>

    <button id="nextTurn" onclick="nextTurn()" style="display: none;">Lanjutkan</button>
  </div>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  text-align: center;
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

button {
  margin: 10px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

#log {
  margin-top: 20px;
  min-height: 50px;
}
let hpPlayer = 5;
let hpEnemy = 5;
let senjataTerpilih = { nama: "", damage: 0 };

function updateStatus() {
  document.getElementById("hpPlayer").innerText = hpPlayer;
  document.getElementById("hpEnemy").innerText = hpEnemy;
}

function pilihSenjata(nama, damage) {
  senjataTerpilih = { nama, damage };
  log(`Kamu memilih ${nama} dengan damage ${damage}.`);
  document.getElementById("weapon-choice").style.display = "none";
  document.getElementById("nextTurn").style.display = "inline-block";
}

function tembakan() {
  return Math.random() < 0.7 ? "Kena!" : "Meleset!";
}

function log(pesan) {
  const logDiv = document.getElementById("log");
  logDiv.innerHTML += `<p>${pesan}</p>`;
  logDiv.scrollTop = logDiv.scrollHeight;
}

function nextTurn() {
  // Pemain Menembak
  let hasil = tembakan();
  if (hasil === "Kena!") {
    hpEnemy -= senjataTerpilih.damage;
    log(`Kamu menembak dengan ${senjataTerpilih.nama}... ${hasil}! (Damage: ${senjataTerpilih.damage})`);
  } else {
    log(`Kamu menembak dengan ${senjataTerpilih.nama}... ${hasil}!`);
  }

  if (hpEnemy <= 0) {
    log("Selamat! Kamu menang!");
    endGame();
    return;
  }

  // Giliran Musuh
  setTimeout(() => {
    let senjataMusuh = pilihSenjataAcak();
    let hasilMusuh = tembakan();
    if (hasilMusuh === "Kena!") {
      hpPlayer -= senjataMusuh.damage;
      log(`Musuh menembak dengan ${senjataMusuh.nama}... ${hasilMusuh}! (Damage: ${senjataMusuh.damage})`);
    } else {
      log(`Musuh menembak dengan ${senjataMusuh.nama}... ${hasilMusuh}!`);
    }

    if (hpPlayer <= 0) {
      log("Kamu kalah... Musuh lebih cepat darimu.");
      endGame();
      return;
    }

    // Cek Power-up
    if (munculPowerUp()) {
      hpPlayer += 1;
      log("Power-up muncul! Nyawamu bertambah 1.");
    }

    // Reset untuk giliran berikutnya
    document.getElementById("weapon-choice").style.display = "block";
    document.getElementById("nextTurn").style.display = "none";
    updateStatus();
  }, 1000);
}

function pilihSenjataAcak() {
  const senjataList = [
    { nama: "Pistol", damage: 1 },
    { nama: "Shotgun", damage: 2 },
    { nama: "Sniper", damage: 3 }
  ];
  return senjataList[Math.floor(Math.random() * senjataList.length)];
}

function munculPowerUp() {
  return Math.random() < 0.3; // 30% kemungkinan muncul power-up
}

function endGame() {
  document.getElementById("weapon-choice").style.display = "none";
  document.getElementById("nextTurn").style.display = "none";
}

updateStatus();
