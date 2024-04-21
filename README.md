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
    </style>
</head>
<body>
    <h1>ElMeasure Customer Validation</h1>
    <form id="apiRequestForm">
        <label for="ipAddress">IP Address:</label>
        <input type="text" id="ipAddress" placeholder="e.g., 202.65.148.204" required>

        <label for="port">Port:</label>
        <input type="text" id="port" placeholder="e.g., 9000" required>

        <label for="customerID">Customer ID:</label>
        <input type="text" id="customerID" placeholder="Enter Customer ID" required>

        <button type="button" onclick="sendApiRequest()">Send Request</button>
    </form>

    <h2>API Response</h2>
    <pre id="apiResponse"></pre>

    <script>
        function sendApiRequest() {
            const ip = document.getElementById('ipAddress').value;
            const port = document.getElementById('port').value;
            const customerId = document.getElementById('customerID').value;
            const url = `http://${ip}:${port}/api/Dashboard/CustomerValidation`;

            fetch(url, {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({ CustomerID: customerId })
            })
            .then(handleResponse)
            .then(showResponse)
            .catch(handleError);
        }

        function handleResponse(response) {
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            } else {
                return response.json();
            }
        }

        function showResponse(data) {
            document.getElementById('apiResponse').textContent = JSON.stringify(data, null, 2);
        }

        function handleError(error) {
            document.getElementById('apiResponse').textContent = 'Error: ' + error.message;
            console.error('Fetch error:', error);
        }
    </script>
</body>
</html>
