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
      letter-spacing: 1px;
    }
    .card {
      background: #111;
      border: 2px solid #ffd700;
      border-radius: 16px;
      box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
      padding: 30px;
      max-width: 400px;
      margin: 40px auto;
    }
    button {
      background: linear-gradient(90deg, #00ff88, #00c878);
      color: #000;
      font-weight: bold;
      border: none;
      border-radius: 12px;
      padding: 12px 18px;
      margin: 8px;
      cursor: pointer;
      transition: all 0.3s;
    }
    button:hover {
      transform: scale(1.05);
      background: linear-gradient(90deg, #ffd700, #ffaa00);
      color: #000;
    }
    input {
      width: 80%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 10px;
      border: 1px solid #00ff88;
      background: #000;
      color: #00ff88;
      text-align: center;
    }
    footer {
      margin-top: 50px;
      color: #777;
      font-size: 14px;
    }
  </style>
</head>

<body>
  <h1>🌾 DNX FARM</h1>
  <p>Stake your <b>DNX</b> to earn rewards from the Dynamix ecosystem!</p>

  <div class="card">
    <button id="connect">🔗 Connect Wallet</button>
    <br />
    <input id="amount" placeholder="Enter DNX amount" />
    <br />
    <button id="deposit">Deposit (Stake)</button>
    <button id="withdraw">Withdraw</button>
  </div>

  <footer>
    Powered by <b>DNX Finance</b> © 2025
  </footer>

  <script>
    let provider, signer, contract;
    // ✅ อัปเดต Address ใหม่แล้ว
    const CONTRACT_ADDRESS = "0x9D9649e1710eF013465C34146874F967EEE73B03";
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
