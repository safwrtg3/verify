<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Key Loader</title>
    <style>
        #loader {
            display: none;
            border: 16px solid #f3f3f3;
            border-top: 16px solid #3498db;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            animation: spin 2s linear infinite;
            margin: 20px auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>

    <h1>Enter your key to load the content</h1>
    <input type="text" id="keyInput" placeholder="Enter Key" />
    <button onclick="verifyKey()">Verify and Load</button>
    <div id="loader"></div>
    <div id="content" style="display: none;">The content has loaded successfully!</div>

    <script>
        async function verifyKey() {
            let key = document.getElementById('keyInput').value;
            if (key === "") {
                alert("Please enter a key.");
                return;
            }

            // Show loader while verifying key
            document.getElementById('loader').style.display = 'block';

            // Make API request to KeyAuth to verify the key
            try {
                const response = await fetch('https://keyauth.com/api/validate', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        appid: 'Tk6DX2prJv', // replace with your KeyAuth app id
                        secret: '0f1eeffc90510c6a6743223ed8efa0df5a631da2659860897bad44df793e92eb', // replace with your KeyAuth secret
                        key: key
                    })
                });
                const data = await response.json();
                if (data.success) {
                    // Key is valid, show content
                    document.getElementById('loader').style.display = 'none';
                    document.getElementById('content').style.display = 'block';
                } else {
                    alert('Invalid key. Please try again.');
                    document.getElementById('loader').style.display = 'none';
                }
            } catch (error) {
                alert('Error occurred: ' + error.message);
                document.getElementById('loader').style.display = 'none';
            }
        }
    </script>

</body>
</html>
