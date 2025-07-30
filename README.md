const fakeAssets = [
  { token: 'Ethereum', symbol: 'ETH', balance: 1.72, value: 5600 },
  { token: 'Tether USD', symbol: 'USDT', balance: 1500, value: 1500 },
  { token: 'BNB', symbol: 'BNB', balance: 3.5, value: 1600 },
];

const fakeTxs = [
  { date: '2025-07-28', type: 'Send', token: 'ETH', amount: '-0.5', status: 'Confirmed' },
  { date: '2025-07-27', type: 'Receive', token: 'USDT', amount: '+500', status: 'Confirmed' },
  { date: '2025-07-26', type: 'Send', token: 'BNB', amount: '-1.0', status: 'Pending' },
];

function handleLogin(event) {
  event.preventDefault();
  document.getElementById('login-container').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  loadAssets();
  loadChart();
  loadTransactions();
  generateQRCode("0x1234ABCD5678EFGH");
}

function loadAssets() {
  const tableBody = document.getElementById('asset-table-body');
  fakeAssets.forEach(asset => {
    const row = document.createElement('tr');
    row.innerHTML = `
      <td>${asset.token}</td>
      <td>${asset.symbol}</td>
      <td>${asset.balance}</td>
      <td>$${asset.value.toLocaleString()}</td>
    `;
    tableBody.appendChild(row);
  });
}

function loadTransactions() {
  const txBody = document.getElementById('tx-table-body');
  fakeTxs.forEach(tx => {
    const row = document.createElement('tr');
    row.innerHTML = `
      <td>${tx.date}</td>
      <td>${tx.type}</td>
      <td>${tx.token}</td>
      <td>${tx.amount}</td>
      <td>${tx.status}</td>
    `;
    txBody.appendChild(row);
  });
}

function loadChart() {
  const ctx = document.getElementById('portfolioChart').getContext('2d');
  new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels: fakeAssets.map(a => a.token),
      datasets: [{
        label: 'Portfolio Value',
        data: fakeAssets.map(a => a.value),
        backgroundColor: ['#3c91e6', '#00c896', '#f5a623']
      }]
    }
  });
}

function generateQRCode(walletAddress) {
  const qrCanvas = document.getElementById("qr-code");
  QRCode.toCanvas(qrCanvas, walletAddress, function (error) {
    if (error) console.error(error);
  });
}
