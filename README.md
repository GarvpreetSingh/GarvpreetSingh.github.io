<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Farewell Party Entry</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-color: #f4f4f9;
        }
        h1 {
            color: #333;
        }
        form, .admin-page {
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            margin: 10px;
        }
        input, button {
            margin: 10px 0;
            padding: 10px;
            width: 100%;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            background-color: #007bff;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .qr-code {
            text-align: center;
        }
        canvas {
            margin: 10px auto;
        }
    </style>
   
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcode/1.4.4/qrcode.min.js">
</head>
<body>
    <h1>Farewell Party Entry Form</h1>
 <form id="entryForm">
    <label for="name">Name:</label>
        <input type="text" id="name" name="name" required>
 <label for="class">Class:</label>
        <input type="text" id="class" name="class" required>
 <label for="section">Section:</label>
        <input type="text" id="section" name="section" required>
    <button type="button" onclick="generateQRCode()">Generate QR Code</button>
    </form>
<div class="qr-code" id="qrCodeSection" style="display: none;">
        <h2>QR Code</h2>
        <canvas id="qrCanvas"></canvas>
    </div>
<div class="admin-page">
        <h1>Admin Sign In</h1>
        <form id="adminLoginForm">
            <label for="adminEmail">Email:</label>
            <input type="email" id="adminEmail" name="adminEmail" required>

 <label for="adminPassword">Password:</label>
            <input type="password" id="adminPassword" name="adminPassword" required>

 <button type="button" onclick="adminLogin()">Login</button>
        </form>

  <div id="scannerSection" style="display: none;">
            <h1>Admin QR Code Scanner</h1>
            <video id="preview" style="width: 100%;"></video>
        </div>
    </div>

  <script src="https://unpkg.com/@zxing/library@1.10.1/umd/index.min.js"></script>
 <script>
        const storedQRCodes = new Map(); 

        function generateQRCode() {
            const name = document.getElementById('name').value;
            const className = document.getElementById('class').value;
            const section = document.getElementById('section').value;

            if (name && className && section) {
                const qrData = `Name: ${name}, Class: ${className}, Section: ${section}`;
                storedQRCodes.set(qrData, true); 

                const canvas = document.getElementById('qrCanvas');
                QRCode.toCanvas(canvas, qrData, { width: 200 }, function (error) {
                    if (error) console.error(error);
                });

                document.getElementById('qrCodeSection').style.display = 'block';
            } else {
                alert('Please fill all fields');
            }
        }

        function adminLogin() {
            const email = document.getElementById('adminEmail').value;
            const password = document.getElementById('adminPassword').value;

            if (email === "kart3541@gmail.com" && password === "angad888") {
                alert('Login successful');
                document.getElementById('scannerSection').style.display = 'block';
                document.getElementById('adminLoginForm').style.display = 'none';

                
                const codeReader = new ZXing.BrowserMultiFormatReader();
                codeReader.decodeFromVideoDevice(null, 'preview', (result, err) => {
                    if (result) {
                        const scannedData = result.text;
                        if (storedQRCodes.has(scannedData)) {
                            alert(`Entry Valid: ${scannedData}`);
                        } else {
                            alert(`Error: Invalid QR Code`);
                        }
                    }
                    if (err && !(err instanceof ZXing.NotFoundException)) {
                        console.error(err);
                    }
                });
            } else {
                alert('Invalid email or password');
            }
        }
    </script>
</body>
</html>

