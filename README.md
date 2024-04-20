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
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok ' + response.statusText);
        }
        return response.json();
    })
    .then(data => {
        document.getElementById('apiResponse').textContent = JSON.stringify(data, null, 2);
    })
    .catch(error => {
        document.getElementById('apiResponse').textContent = 'Error: ' + error.toString();
    });
}
