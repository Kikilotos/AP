<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>💰 Phang & Avi's Financial Tracker</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            min-height: 100vh;
            padding: 20px;
            animation: gradientShift 10s ease infinite;
        }

        @keyframes gradientShift {
            0%, 100% { background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%); }
            50% { background: linear-gradient(135deg, #f093fb 0%, #667eea 50%, #764ba2 100%); }
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            box-shadow: 0 32px 64px rgba(0,0,0,0.15);
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 40px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grain" width="100" height="100" patternUnits="userSpaceOnUse"><circle cx="25" cy="25" r="1" fill="white" opacity="0.1"/><circle cx="75" cy="75" r="1" fill="white" opacity="0.1"/><circle cx="50" cy="10" r="1" fill="white" opacity="0.1"/><circle cx="10" cy="90" r="1" fill="white" opacity="0.1"/></pattern></defs><rect width="100" height="100" fill="url(%23grain)"/></svg>');
            opacity: 0.3;
        }
        
        .header h1 {
            font-size: 3em;
            font-weight: 700;
            margin-bottom: 10px;
            text-shadow: 0 4px 12px rgba(0,0,0,0.2);
            position: relative;
            z-index: 1;
        }
        
        .header p {
            font-size: 1.3em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }
        
        .content {
            padding: 40px;
        }
        
        .section {
            margin-bottom: 40px;
            padding: 30px;
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.3);
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .section:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 48px rgba(0,0,0,0.15);
        }
        
        .section h2 {
            margin: 0 0 25px 0;
            color: #333;
            font-size: 1.8em;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        
        .editable-table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            background: white;
            border-radius: 16px;
            overflow: hidden;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
        }
        
        .editable-table th, .editable-table td {
            padding: 16px 20px;
            text-align: left;
            border-bottom: 1px solid rgba(0,0,0,0.05);
        }
        
        .editable-table th {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            font-weight: 600;
            font-size: 1.1em;
            text-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        
        .editable-table tr:nth-child(even) {
            background: rgba(102, 126, 234, 0.03);
        }
        
        .editable-table tr:hover {
            background: rgba(102, 126, 234, 0.08);
            transition: background 0.3s ease;
        }
        
        .editable-input {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid rgba(102, 126, 234, 0.2);
            border-radius: 12px;
            font-size: 1em;
            transition: all 0.3s ease;
            background: rgba(255, 255, 255, 0.9);
        }
        
        .editable-input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 4px rgba(102, 126, 234, 0.1);
            transform: translateY(-2px);
        }
        
        .calculated-value {
            font-weight: 700;
            color: #667eea;
            font-size: 1.2em;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .total-row {
            background: linear-gradient(135deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1)) !important;
            font-weight: 600;
        }
        
        .total-row td {
            border-top: 3px solid #667eea;
        }
        
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 24px;
            margin: 24px 0;
        }
        
        .card {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            padding: 24px;
            border-radius: 16px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            border: 1px solid rgba(255, 255, 255, 0.3);
            transition: all 0.3s ease;
        }

        .card:hover {
            transform: translateY(-6px);
            box-shadow: 0 16px 48px rgba(0,0,0,0.15);
        }
        
        .card h3 {
            margin: 0 0 16px 0;
            color: #333;
            font-size: 1.2em;
            font-weight: 600;
        }
        
        .amount {
            font-size: 2.2em;
            font-weight: 700;
            color: #667eea;
            margin: 8px 0;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .percentage {
            font-size: 1.1em;
            color: #666;
            margin: 8px 0;
            font-weight: 500;
        }
        
        .progress-bar {
            width: 100%;
            height: 8px;
            background: rgba(102, 126, 234, 0.2);
            border-radius: 4px;
            overflow: hidden;
            margin: 12px 0;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            border-radius: 4px;
            transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .risk-low { border-left: 4px solid #4CAF50; }
        .risk-moderate { border-left: 4px solid #FF9800; }
        .risk-high { border-left: 4px solid #F44336; }
        
        .update-btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 16px 32px;
            border-radius: 12px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: 600;
            margin: 16px 0;
            transition: all 0.3s ease;
            box-shadow: 0 4px 16px rgba(102, 126, 234, 0.3);
        }
        
        .update-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 24px rgba(102, 126, 234, 0.4);
        }

        .update-btn:active {
            transform: translateY(0);
        }
        
        .status-message {
            padding: 16px;
            border-radius: 12px;
            margin: 16px 0;
            text-align: center;
            font-weight: 600;
            transition: all 0.3s ease;
        }
        
        .status-success {
            background: linear-gradient(135deg, #d4edda, #c3e6cb);
            color: #155724;
            border: 1px solid #c3e6cb;
            animation: slideIn 0.5s ease;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .emoji {
            font-size: 1.3em;
            margin-right: 8px;
        }

        .auto-save-indicator {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(102, 126, 234, 0.9);
            color: white;
            padding: 12px 20px;
            border-radius: 8px;
            font-size: 0.9em;
            opacity: 0;
            transition: opacity 0.3s ease;
            z-index: 1000;
        }

        .auto-save-indicator.show {
            opacity: 1;
        }

        .names-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 20px;
            background: linear-gradient(135deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1));
            border-radius: 16px;
            border: 1px solid rgba(102, 126, 234, 0.2);
        }

        .name-tag {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: 600;
            font-size: 1.1em;
            text-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }

        @media (max-width: 768px) {
            .header h1 { font-size: 2.2em; }
            .dashboard-grid { grid-template-columns: 1fr; }
            .names-header { flex-direction: column; gap: 12px; }
        }
    </style>
</head>
<body>
    <div class="auto-save-indicator" id="autoSaveIndicator">💾 Auto-saved</div>
    
    <div class="container">
        <div class="header">
            <h1>💰 Phang & Avi's Financial Tracker</h1>
            <p>📊 Building Wealth Together</p>
        </div>
        
        <div class="content">
            <div class="names-header">
                <div class="name-tag">👨‍💼 Phang</div>
                <div style="font-size: 1.5em; color: #667eea;">💕</div>
                <div class="name-tag">👩‍💼 Avi</div>
            </div>

            <!-- Editable Data Table -->
            <div class="section">
                <h2><span class="emoji">📝</span>Portfolio Overview</h2>
                <table class="editable-table">
                    <thead>
                        <tr>
                            <th>Date</th>
                            <th>💰 Savings (Wealthfront)</th>
                            <th>📈 Brokerage (VOO)</th>
                            <th>₿ Bitcoin</th>
                            <th>🔷 Other Crypto</th>
                            <th>📊 Total</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><input type="date" class="editable-input" id="date1" value="2025-07-12"></td>
                            <td><input type="number" class="editable-input" id="savings1" value="2000" min="0" step="0.01"></td>
                            <td><input type="number" class="editable-input" id="brokerage1" value="2000" min="0" step="0.01"></td>
                            <td><input type="number" class="editable-input" id="bitcoin1" value="400" min="0" step="0.01"></td>
                            <td><input type="number" class="editable-input" id="othercrypto1" value="0" min="0" step="0.01"></td>
                            <td class="calculated-value" id="total1">$4,400.00</td>
                        </tr>
                        <tr class="total-row">
                            <td><strong>Portfolio Total</strong></td>
                            <td class="calculated-value" id="totalSavings">$2,000.00</td>
                            <td class="calculated-value" id="totalBrokerage">$2,000.00</td>
                            <td class="calculated-value" id="totalBitcoin">$400.00</td>
                            <td class="calculated-value" id="totalOtherCrypto">$0.00</td>
                            <td class="calculated-value" id="grandTotal">$4,400.00</td>
                        </tr>
                    </tbody>
                </table>
                
                <button class="update-btn" onclick="updateCalculations()">🔄 Update Calculations</button>
                <div id="statusMessage"></div>
            </div>

            <!-- Monthly Tracking -->
            <div class="section">
                <h2><span class="emoji">📅</span>Monthly Tracking</h2>
                <table class="editable-table">
                    <thead>
                        <tr>
                            <th>Category</th>
                            <th>👨‍💼 Phang's Contribution</th>
                            <th>👩‍💼 Avi's Contribution</th>
                            <th>📊 Total</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><strong>This Month</strong></td>
                            <td><input type="number" class="editable-input" id="phangMonthly" value="0" min="0" step="0.01"></td>
                            <td><input type="number" class="editable-input" id="aviMonthly" value="0" min="0" step="0.01"></td>
                            <td class="calculated-value" id="totalMonthly">$0.00</td>
                        </tr>
                        <tr>
                            <td><strong>Monthly Goal</strong></td>
                            <td colspan="2"><input type="number" class="editable-input" id="monthlyGoal" value="1000" min="0" step="0.01"></td>
                            <td class="calculated-value" id="goalProgress">0.0%</td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <!-- Individual Contributions -->
            <div class="section">
                <h2><span class="emoji">👥</span>Individual Contributions</h2>
                <div class="dashboard-grid">
                    <div class="card">
                        <h3>👨‍💼 Phang's Contributions</h3>
                        <div style="margin: 15px 0;">
                            <div style="margin: 8px 0;">💰 Savings: $<input type="number" class="editable-input" id="phangSavings" value="1000" min="0" step="0.01" style="width: 120px; display: inline;"></div>
                            <div style="margin: 8px 0;">📈 Brokerage: $<input type="number" class="editable-input" id="phangBrokerage" value="1000" min="0" step="0.01" style="width: 120px; display: inline;"></div>
                            <div style="margin: 8px 0;">₿ Bitcoin: $<input type="number" class="editable-input" id="phangBitcoin" value="200" min="0" step="0.01" style="width: 120px; display: inline;"></div>
                        </div>
                        <div style="border-top: 2px solid #667eea; padding-top: 15px; margin-top: 15px;">
                            <strong>Phang's Total: <span class="amount" id="phangTotal">$2,200.00</span></strong>
                        </div>
                        <div class="percentage" id="phangPercentage">50.0%</div>
                        <div class="progress-bar">
                            <div class="progress-fill" id="phangProgress" style="width: 50%"></div>
                        </div>
                    </div>
                    <div class="card">
                        <h3>👩‍💼 Avi's Contributions</h3>
                        <div style="margin: 15px 0;">
                            <div style="margin: 8px 0;">💰 Savings: $<input type="number" class="editable-input" id="aviSavings" value="1000" min="0" step="0.01" style="width: 120px; display: inline;"></div>
                            <div style="margin: 8px 0;">📈 Brokerage: $<input type="number" class="editable-input" id="aviBrokerage" value="1000" min="0" step="0.01" style="width: 120px; display: inline;"></div>
                            <div style="margin: 8px 0;">₿ Bitcoin: $<input type="number" class="editable-input" id="aviBitcoin" value="200" min="0" step="0.01" style="width: 120px; display: inline;"></div>
                        </div>
                        <div style="border-top: 2px solid #667eea; padding-top: 15px; margin-top: 15px;">
                            <strong>Avi's Total: <span class="amount" id="aviTotal">$2,200.00</span></strong>
                        </div>
                        <div class="percentage" id="aviPercentage">50.0%</div>
                        <div class="progress-bar">
                            <div class="progress-fill" id="aviProgress" style="width: 50%"></div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Dashboard -->
            <div class="section">
                <h2><span class="emoji">📊</span>Portfolio Dashboard</h2>
                <div class="dashboard-grid">
                    <div class="card risk-low">
                        <h3>🛡️ Conservative (Savings)</h3>
                        <div class="amount" id="savingsAmount">$2,000.00</div>
                        <div class="percentage" id="savingsPercentage">45.5%</div>
                        <div class="progress-bar">
                            <div class="progress-fill" id="savingsProgress" style="width: 45.5%"></div>
                        </div>
                    </div>
                    <div class="card risk-moderate">
                        <h3>⚖️ Moderate (Brokerage)</h3>
                        <div class="amount" id="brokerageAmount">$2,000.00</div>
                        <div class="percentage" id="brokeragePercentage">45.5%</div>
                        <div class="progress-bar">
                            <div class="progress-fill" id="brokerageProgress" style="width: 45.5%"></div>
                        </div>
                    </div>
                    <div class="card risk-high">
                        <h3>🎲 High Risk (Crypto)</h3>
                        <div class="amount" id="cryptoAmount">$400.00</div>
                        <div class="percentage" id="cryptoPercentage">9.1%</div>
                        <div class="progress-bar">
                            <div class="progress-fill" id="cryptoProgress" style="width: 9.1%"></div>
                        </div>
                    </div>
                    <div class="card">
                        <h3>🎯 Monthly Goal Progress</h3>
                        <div class="amount" id="monthlyProgressAmount">$0.00</div>
                        <div class="percentage" id="monthlyProgressPercentage">0.0%</div>
                        <div class="progress-bar">
                            <div class="progress-fill" id="monthlyProgressBar" style="width: 0%"></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Auto-save functionality
        let autoSaveTimeout;
        const STORAGE_KEY = 'phang_avi_financial_data';

        // Load saved data on page load
        window.addEventListener('load', function() {
            loadSavedData();
            updateCalculations();
        });

        // Auto-save when inputs change
        document.addEventListener('input', function(e) {
            if (e.target.classList.contains('editable-input')) {
                updateCalculations();
                autoSave();
            }
        });

        function autoSave() {
            clearTimeout(autoSaveTimeout);
            autoSaveTimeout = setTimeout(() => {
                saveData();
                showAutoSaveIndicator();
            }, 1000);
        }

        function saveData() {
            const data = {};
            const inputs = document.querySelectorAll('.editable-input');
            inputs.forEach(input => {
                data[input.id] = input.value;
            });
            
            // Save to localStorage
            localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
        }

        function loadSavedData() {
            const savedData = localStorage.getItem(STORAGE_KEY);
            if (savedData) {
                const data = JSON.parse(savedData);
                Object.keys(data).forEach(id => {
                    const input = document.getElementById(id);
                    if (input) {
                        input.value = data[id];
                    }
                });
            }
        }

        function showAutoSaveIndicator() {
            const indicator = document.getElementById('autoSaveIndicator');
            indicator.classList.add('show');
            setTimeout(() => {
                indicator.classList.remove('show');
            }, 2000);
        }

        function updateCalculations() {
            // Get input values
            const savings = parseFloat(document.getElementById('savings1').value) || 0;
            const brokerage = parseFloat(document.getElementById('brokerage1').value) || 0;
            const bitcoin = parseFloat(document.getElementById('bitcoin1').value) || 0;
            const otherCrypto = parseFloat(document.getElementById('othercrypto1').value) || 0;
            
            const phangMonthly = parseFloat(document.getElementById('phangMonthly').value) || 0;
            const aviMonthly = parseFloat(document.getElementById('aviMonthly').value) || 0;
            const monthlyGoal = parseFloat(document.getElementById('monthlyGoal').value) || 1000;
            
            const phangSavings = parseFloat(document.getElementById('phangSavings').value) || 0;
            const phangBrokerage = parseFloat(document.getElementById('phangBrokerage').value) || 0;
            const phangBitcoin = parseFloat(document.getElementById('phangBitcoin').value) || 0;
            
            const aviSavings = parseFloat(document.getElementById('aviSavings').value) || 0;
            const aviBrokerage = parseFloat(document.getElementById('aviBrokerage').value) || 0;
            const aviBitcoin = parseFloat(document.getElementById('aviBitcoin').value) || 0;
            
            // Calculate totals
            const total = savings + brokerage + bitcoin + otherCrypto;
            const totalMonthly = phangMonthly + aviMonthly;
            const goalProgress = (totalMonthly / monthlyGoal) * 100;
            
            const phangTotal = phangSavings + phangBrokerage + phangBitcoin;
            const aviTotal = aviSavings + aviBrokerage + aviBitcoin;
            const combinedTotal = phangTotal + aviTotal;
            
            // Update main totals
            document.getElementById('total1').textContent = formatCurrency(total);
            document.getElementById('totalSavings').textContent = formatCurrency(savings);
            document.getElementById('totalBrokerage').textContent = formatCurrency(brokerage);
            document.getElementById('totalBitcoin').textContent = formatCurrency(bitcoin);
            document.getElementById('totalOtherCrypto').textContent = formatCurrency(otherCrypto);
            document.getElementById('grandTotal').textContent = formatCurrency(total);
            
            // Update monthly tracking
            document.getElementById('totalMonthly').textContent = formatCurrency(totalMonthly);
            document.getElementById('goalProgress').textContent = goalProgress.toFixed(1) + '%';
            
            // Update individual contributions
            document.getElementById('phangTotal').textContent = formatCurrency(phangTotal);
            document.getElementById('aviTotal').textContent = formatCurrency(aviTotal);
            
            const phangPercentage = combinedTotal > 0 ? (phangTotal / combinedTotal * 100).toFixed(1) : 0;
            const aviPercentage = combinedTotal > 0 ? (aviTotal / combinedTotal * 100).toFixed(1) : 0;
            
            document.getElementById('phangPercentage').textContent = phangPercentage + '%';
            document.getElementById('aviPercentage').textContent = aviPercentage + '%';
            
            // Update progress bars for individuals
            document.getElementById('phangProgress').style.width = phangPercentage + '%';
            document.getElementById('aviProgress').style.width = aviPercentage + '%';
            
            // Update dashboard
            const savingsPercentage = total > 0 ? (savings / total * 100).toFixed(1) : 0;
            const brokeragePercentage = total > 0 ? (brokerage / total * 100).toFixed(1) : 0;
            const cryptoPercentage = total > 0 ? ((bitcoin + otherCrypto) / total * 100).toFixed(1) : 0;
            
            document.getElementById('savingsAmount').textContent = formatCurrency(savings);
            document.getElementById('brokerageAmount').textContent = formatCurrency(brokerage);
            document.getElementById('cryptoAmount').textContent = formatCurrency(bitcoin + otherCrypto);
            document.getElementById('monthlyProgressAmount').textContent = formatCurrency(totalMonthly);
            
            document.getElementById('savingsPercentage').textContent = savingsPercentage + '%';
            document.getElementById('brokeragePercentage').textContent = brokeragePercentage + '%';
            document.getElementById('cryptoPercentage').textContent = cryptoPercentage + '%';
            document.getElementById('monthlyProgressPercentage').textContent = goalProgress.toFixed(1) + '%';
            
            // Update progress bars
            document.getElementById('savingsProgress').style.width = savingsPercentage + '%';
            document.getElementById('brokerageProgress').style.width = brokeragePercentage + '%';
            document.getElementById('cryptoProgress').style.width = cryptoPercentage + '%';
            document.getElementById('monthlyProgressBar').style.width = Math.min(goalProgress, 100) + '%';
            
            // Show success message
            const statusMessage = document.getElementById('statusMessage');
            statusMessage.innerHTML = '<div class="status-success">✅ Calculations updated successfully!</div>';
            setTimeout(() => { statusMessage.innerHTML = ''; }, 3000);
        }
        
        function formatCurrency(amount) {
            return '$' + amount.toLocaleString('en-US', { 
                minimumFractionDigits: 2, 
                maximumFractionDigits: 2 
            });
        }
    </script>
</body>
</html>
