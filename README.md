<script>
    function sendApiRequest() {
        const customerId = document.getElementById('customerID').value;
        const url = `http://localhost:3000/api/Dashboard/CustomerValidation`; // Local proxy URL

        fetch(url, {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({ CustomerID: customerId })
        })
        .then(response => {
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            return response.json();
        })
        .then(data => {
            document.getElementById('apiResponse').textContent = JSON.stringify(data, null, 2);
        })
        .catch(error => {
            document.getElementById('apiResponse').textContent = 'Error: ' + error.message;
            console.error('Fetch error:', error);
        });
    }
</script>
