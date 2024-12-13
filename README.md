# index.html
dolar billete
<html><head><base href="." />
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Minado de Dollar</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.7.0/chart.min.js"></script>
<style>
body {
    margin: 0;
    padding: 10px;
    background: #1a1a1a;
    color: #fff;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.container {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
    padding: 15px;
    background: #2a2a2a;
    border-radius: 15px;
    box-shadow: 0 0 20px rgba(0,0,0,0.5);
    box-sizing: border-box;
}

h1 {
    font-size: 1.8rem;
    text-align: center;
    margin: 10px 0;
}

.mining-stats {
    display: grid;
    grid-template-columns: 1fr;
    gap: 10px;
    margin-bottom: 15px;
}

@media (min-width: 480px) {
    .mining-stats {
        grid-template-columns: repeat(3, 1fr);
    }
}

.stat-box {
    background: #333;
    padding: 12px;
    border-radius: 10px;
    text-align: center;
}

.stat-value {
    font-size: 20px;
    font-weight: bold;
    color: #4CAF50;
}

.stat-label {
    font-size: 12px;
    color: #888;
}

#miningButton {
    width: 100%;
    padding: 15px;
    background: linear-gradient(45deg, #4CAF50, #45a049);
    border: none;
    border-radius: 10px;
    color: white;
    font-size: 16px;
    cursor: pointer;
    margin-bottom: 15px;
    transition: transform 0.2s;
    -webkit-tap-highlight-color: transparent;
}

#miningButton:hover {
    transform: scale(1.02);
}

#miningButton:active {
    transform: scale(0.98);
}

#miningButton:disabled {
    background: #666;
    cursor: not-allowed;
}

.mining-animation {
    display: flex;
    justify-content: center;
    margin: 15px 0;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.chart-container {
    margin-top: 15px;
    background: #333;
    padding: 10px;
    border-radius: 10px;
    width: 100%;
    height: 200px;
}

canvas {
    width: 100% !important;
    height: 100% !important;
}
</style>
</head>
<body>
<div class="container">
    <h1>💰 Minado de Dollar</h1>
    
    <div class="mining-stats">
        <div class="stat-box">
            <div class="stat-value">¢<span id="totalMined">0.00</span></div>
            <div class="stat-label">Total Centavos Minados</div>
        </div>
        <div class="stat-box">
            <div class="stat-value"><span id="hashRate">0.00</span> H/s</div>
            <div class="stat-label">Tasa de Hash</div>
        </div>
        <div class="stat-box">
            <div class="stat-value">¢<span id="perHour">0.00</span></div>
            <div class="stat-label">Centavos/Hora</div>
        </div>
    </div>

    <button id="miningButton">Comenzar Minería</button>

    <div class="mining-animation">
        <svg width="80" height="80" viewBox="0 0 100 100" id="miningGear">
            <path d="M50 25L55 35L65 35L60 45H40L35 35L45 35Z" fill="#4CAF50"/>
            <circle cx="50" cy="50" r="20" stroke="#4CAF50" stroke-width="4" fill="none"/>
        </svg>
    </div>

    <div class="chart-container">
        <canvas id="miningChart"></canvas>
    </div>
</div>

<script>
let isMining = false;
let totalMined = 0;
let miningInterval;
let chartData = {
    labels: [],
    earnings: []
};

const miningGear = document.getElementById('miningGear');
const chart = new Chart(document.getElementById('miningChart').getContext('2d'), {
    type: 'line',
    data: {
        labels: chartData.labels,
        datasets: [{
            label: 'Ganancias de Minería (Centavos)',
            data: chartData.earnings,
            borderColor: '#4CAF50',
            tension: 0.4
        }]
    },
    options: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
            y: {
                beginAtZero: true,
                grid: {
                    color: '#444'
                },
                ticks: {
                    font: {
                        size: 10
                    }
                }
            },
            x: {
                grid: {
                    color: '#444'
                },
                ticks: {
                    font: {
                        size: 10
                    },
                    maxRotation: 45,
                    minRotation: 45
                }
            }
        },
        plugins: {
            legend: {
                labels: {
                    color: '#fff',
                    font: {
                        size: 10
                    }
                }
            }
        }
    }
});

document.getElementById('miningButton').addEventListener('click', function() {
    if (!isMining) {
        startMining();
        this.textContent = 'Detener Minería';
        miningGear.style.animation = 'spin 8s linear infinite'; // Even slower gear animation
    } else {
        stopMining();
        this.textContent = 'Comenzar Minería';
        miningGear.style.animation = 'none';
    }
    isMining = !isMining;
});

function startMining() {
    miningInterval = setInterval(() => {
        // Further reduced earnings and slower rate
        const earning = Math.random() * 0.05 + 0.005; // Reduced from 0.1 to 0.05
        totalMined += earning;
        
        document.getElementById('totalMined').textContent = totalMined.toFixed(2);
        document.getElementById('hashRate').textContent = (Math.random() * 10 + 5).toFixed(2); // Reduced from 20 to 10
        document.getElementById('perHour').textContent = (earning * 3600).toFixed(2);

        const now = new Date();
        chartData.labels.push(now.toLocaleTimeString());
        chartData.earnings.push(earning);
        
        if (chartData.labels.length > 10) {
            chartData.labels.shift();
            chartData.earnings.shift();
        }
        
        chart.update();
    }, 4000); // Increased from 2000ms to 4000ms (4 seconds)
}

function stopMining() {
    clearInterval(miningInterval);
}
</script>
</body></html>
