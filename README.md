<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>API Request Interface</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        label, input, button { display: block; margin: 10px 0; }
        input, button { width: 300px; padding: 8px; }
        button { cursor: pointer; background-color: #4CAF50; color: white; border: none; }
        button:hover { background-color: #45a049; }
        pre { background-color: #f4f4f4; border: 1px solid #ddd; padding: 10px; }
        .section { margin-bottom: 40px; }
    </style>
</head>
<body>
    <div class="section">
        <h1>ElMeasure Meter Reading Validation</h1>
        <form id="meterReadingForm">
            <label for="meterIpAddress">IP Address:</label>
            <input type="text" id="meterIpAddress" placeholder="e.g., 43.230.67.26" required>

            <label for="meterPort">Port:</label>
            <input type="text" id="meterPort" placeholder="e.g., 9001" required>

            <label for="meterSerialNumber">Meter Serial Number:</label>
            <input type="text" id="meterSerialNumber" placeholder="Enter Meter Serial Number" required>

            <label for="meterCustomerID">Customer ID:</label>
            <input type="text" id="meterCustomerID" placeholder="Enter Customer ID" required>

            <button type="button" onclick="sendMeterReadingRequest()">Send Request</button>
        </form>
        <h2>Meter Reading API Response</h2>
        <pre id="meterApiResponse"></pre>
    </div>

    <div class="section">
        <h1>ElMeasure Customer Validation</h1>
        <form id="customerValidationForm">
            <label for="customerIpAddress">IP Address:</label>
            <input type="text" id="customerIpAddress" placeholder="e.g., 202.65.148.204" required>

            <label for="customerPort">Port:</label>
            <input type="text" id="customerPort" placeholder="e.g., 9000" required>

            <label for="validationCustomerID">Customer ID:</label>
            <input type="text" id="validationCustomerID" placeholder="Enter Customer ID" required>

            <button type="button" onclick="sendCustomerValidationRequest()">Send Request</button>
        </form>
        <h2>Customer Validation API Response</h2>
        <pre id="customerApiResponse"></pre>
    </div>

    <script>
        function sendMeterReadingRequest() {
            const ip = document.getElementById('meterIpAddress').value;
            const port = document.getElementById('meterPort').value;
            const serialNumber = document.getElementById('meterSerialNumber').value;
            const customerId = document.getElementById('meterCustomerID').value;
            const url = `http://${ip}:${port}/api/Dashboard/MeterReading`;

            fetch(url, {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({ MeterSerialNumber: serialNumber, CustomerID: customerId })
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById('meterApiResponse').textContent = JSON.stringify(data, null, 2);
            })
            .catch(error => {
                document.getElementById('meterApiResponse').textContent = 'Error: ' + error.message;
            });
        }

        function sendCustomerValidationRequest() {
            const ip = document.getElementById('customerIpAddress').value;
            const port = document.getElementById('customerPort').value;
            const customerId = document.getElementById('validationCustomerID').value;
            const url = `http://${ip}:${port}/api/Dashboard/CustomerValidation`;

            fetch(url, {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({ CustomerID: customerId })
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById('customerApiResponse').textContent = JSON.stringify(data, null, 2);
            })
            .catch(error => {
                document.getElementById('customerApiResponse').textContent = 'Error: ' + error.message;
            });
        }
    </script>
</body>
</html>
