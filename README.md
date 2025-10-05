<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DNX Farm</title>
  <script src="https://cdn.jsdelivr.net/npm/ethers@6.3.0/dist/ethers.min.js"></script>

  <style>
    body {
      background: radial-gradient(circle at top, #000000 40%, #1a1a1a 100%);
      color: #00ff88;
      font-family: "Poppins", sans-serif;
      text-align: center;
      padding: 60px 20px;
    }

    h1 {
      color: #ffd700;
      letter-spacing: 2px;
      font-size: 2.5em;
      margin-bottom: 10px;
    }

    .subtitle {
      color: #ccc;
      font-size: 1.1em;
      margin-bottom: 40px;
    }

    .card {
      background: #111;
      border: 2px solid #ffd700;
      border-radius: 18px;
      box-shadow: 0 0 25px rgba(255, 215, 0, 0.25);
      padding: 30px;
      max-width: 420px;
      margin: 0 auto;
    }

    .reward-box {
      background: linear-gradient(90deg, #ffd70022, #00ff8822);
      border: 1px solid #00ff88;
      border-radius: 12px;
      padding: 10px;
      margin: 10px 0 20px;
      color: #00ff88;
      font-weight: 500;
    }

    button {
      background: linear-gradient(90deg, #00ff88, #00c878);
      color: #000;
      font-weight: bold;
      border: none;
      border-radius: 12px;
      padding: 12px 20px;
      margin: 8px;
      cursor: pointer;
      transition: all 0.3s;
    }

    button:hover {
      transform: scale(1.07);
      background: linear-gradient(90deg, #ffd700, #ffaa00);
      color: #000;
    }

    input {
      width: 80%;
      padding: 12px;
      margin: 10px 0;
      border-radius: 10px;
      border: 1px solid #00ff88;
      background: #000;
      color: #00ff88;
      text-align: center;
      font-size: 1em;
    }

    .link-btn {
      background: none;
      color: #ffd700;
      border: 1px solid #ffd700;
      padding: 10px 16px;
      border-radius: 10px;
      text-decoration: none;
      font-weight: bold;
      margin: 6px;
      display: inline-block;
      transition: 0.3s;
    }

    .link-btn:hover {
      background: #ffd700;
      color: #000;
    }

    footer {
      margin-top: 60px;
      color: #777;
      font-size: 14px;
    }

    img.logo {
      width: 100px;
      margin-bottom: 20px;
      filter: drop-shadow(0 0 12px #ffd700aa);
    }
  </style>
</head>

<body>
  <img class="logo" src="https://raw.githubusercontent.com/JESSADA785/dnxfinance.github.io/main/dnx_logo.png" alt="DNX Logo" />
  <h1>🌾 DNX FARM</h1>
  <div class="subtitle">Stake DNX to earn daily rewards in the Dynamix ecosystem</div>

  <div class="card">
    <div class="reward-box">💰 Reward: <b>1% Daily</b></div>

    <button id="connect">🔗 Connect Wallet</button>
    <br /><br />

    <input id="amount" placeholder="Enter DNX amount" />
    <br />
    <button id="deposit">Deposit (Stake)</button>
    <button id="withdraw">Withdraw</button>
    <br /><br />

    <a class="link-btn" href="https://bscscan.com/token/0x7DB860CC9D7f64Ed1B39de6621ebc94440C8dc62" target="_blank">🔍 View on BscScan</a>
    <a class="link-btn" href="https://pancakeswap.finance/info/pairs" target="_blank">📈 View Chart (DNX/BNB)</a>
  </div>

  <footer>
    Powered by <b>DNX Finance</b> © 2025  
  </footer>

  <script>
    let provider, signer, contract;
    const CONTRACT_ADDRESS = "0x9D9649e1710eF013465C34146874F967EEE73B03"; // ✅ DNXMasterChef
    const ABI = [
      "function deposit(uint256 _amount) external",
      "function withdraw(uint256 _amount) external"
    ];

    async function connect() {
      if (!window.ethereum) return alert("⚠️ Please install MetaMask first!");
      provider = new ethers.BrowserProvider(window.ethereum);
      await provider.send("eth_requestAccounts", []);
      signer = await provider.getSigner();
      contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, signer);
      alert("✅ Wallet connected: " + (await signer.getAddress()));
    }

    async function deposit() {
      try {
        const amt = document.getElementById("amount").value;
        const tx = await contract.deposit(ethers.parseUnits(amt, 18));
        await tx.wait();
        alert("✅ Staked " + amt + " DNX successfully!");
      } catch (err) {
        alert("❌ Transaction failed: " + err.message);
      }
    }

    async function withdraw() {
      try {
        const amt = document.getElementById("amount").value;
        const tx = await contract.withdraw(ethers.parseUnits(amt, 18));
        await tx.wait();
        alert("✅ Withdrawn " + amt + " DNX successfully!");
      } catch (err) {
        alert("❌ Transaction failed: " + err.message);
      }
    }

    document.getElementById("connect").onclick = connect;
    document.getElementById("deposit").onclick = deposit;
    document.getElementById("withdraw").onclick = withdraw;
  </script>
</body>
</html>
