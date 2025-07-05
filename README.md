# callrate-

<!DOCTYPE html>
<html>
<head>
    <title>Call Cost Calculator (BDT)</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 500px; margin: 0 auto; padding: 20px; }
        .calculator { border: 1px solid #ccc; padding: 20px; border-radius: 5px; }
        input, select { padding: 8px; margin: 5px 0; width: 100%; box-sizing: border-box; }
        button { background-color: #4CAF50; color: white; padding: 10px; border: none; cursor: pointer; width: 100%; }
        .result { margin-top: 20px; padding: 10px; background-color: #f8f8f8; border-radius: 5px; }
        .note { font-size: 0.9em; color: #666; margin-bottom: 15px; }
        .unit-container { display: flex; align-items: center; }
        .unit-container input { flex: 1; }
        .unit-container span { margin-left: 10px; }
    </style>
</head>
<body>
    <div class="calculator">
        <h2>Call Cost Calculator (BDT)</h2>
        <div class="note">Note: Enter call rate in paisa (100 paisa = 1 BDT)</div>
        
        <div>
            <label for="rateType">Billing Type:</label>
            <select id="rateType">
                <option value="minute">Per Minute</option>
                <option value="second">Per Second</option>
            </select>
        </div>
        <div>
            <label for="rate">Call Rate (paisa):</label>
            <input type="number" id="rate" step="0.01" min="0">
        </div>
        <div class="unit-container">
            <label for="unit" id="unitLabel">Per:</label>
            <input type="number" id="unit" min="0.01" step="0.01" value="1">
            <span id="unitSuffix">minute(s)</span>
        </div>
        <div>
            <label for="duration">Call Duration (minutes):</label>
            <input type="number" id="duration" step="0.01" min="0">
        </div>
        <button onclick="calculate()">Calculate Cost</button>
        <div id="result" class="result" style="display: none;">
            <h3>Calculation Result:</h3>
            <p id="rateDisplay"></p>
            <p id="durationDisplay"></p>
            <p id="costDisplay"></p>
        </div>
    </div>

    <script>
        document.getElementById('rateType').addEventListener('change', function() {
            const isMinute = this.value === 'minute';
            document.getElementById('unitLabel').textContent = 'Per:';
            document.getElementById('unitSuffix').textContent = isMinute ? 'minute(s)' : 'second(s)';
            document.getElementById('unit').value = isMinute ? '1' : '10'; // Default to 10 seconds
        });

        function calculate() {
            const rateType = document.getElementById('rateType').value;
            const ratePaisa = parseFloat(document.getElementById('rate').value);
            const unit = parseFloat(document.getElementById('unit').value);
            const duration = parseFloat(document.getElementById('duration').value);
            
            if (isNaN(ratePaisa) || isNaN(unit) || isNaN(duration) || unit <= 0) {
                alert("Please enter valid numbers in all fields");
                return;
            }
            
            // Convert paisa to taka
            const rateTaka = ratePaisa / 100;
            
            let totalCost;
            if (rateType === 'minute') {
                totalCost = (rateTaka / unit) * duration;
            } else {
                totalCost = (rateTaka / unit) * (duration * 60);
            }
            
            document.getElementById('rateDisplay').textContent = 
                `Call rate: ${ratePaisa} paisa (${rateTaka.toFixed(2)} BDT) per ${unit} ${rateType === 'minute' ? 'minute(s)' : 'second(s)'}`;
            document.getElementById('durationDisplay').textContent = 
                `Call duration: ${duration} minutes`;
            document.getElementById('costDisplay').textContent = 
                `Total cost: ${totalCost.toFixed(2)} BDT`;
            document.getElementById('result').style.display = 'block';
        }
    </script>
</body>
</html>
