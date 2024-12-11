<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IMEI Checker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: #f9f9f9;
            text-align: center;
        }
        .container {
            max-width: 600px;
            margin: 50px auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            color: #333;
        }
        input, select {
            width: 90%;
            padding: 10px;
            margin: 15px 0;
            font-size: 16px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        button {
            padding: 10px 20px;
            background: #007bff;
            color: white;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background: #0056b3;
        }
        .result {
            margin-top: 20px;
            padding: 10px;
            background: #f1f1f1;
            border-radius: 5px;
        }
        .brand-section {
            margin-top: 30px;
        }
        .brand-section h2 {
            color: #333;
        }
        #captcha {
            margin-top: 10px;
        }

        /* Responsive design */
        @media screen and (max-width: 600px) {
            .container {
                width: 100%;
                padding: 15px;
            }
            input, select, button {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>IMEI Checker</h1>
        <input type="text" id="imei" placeholder="Enter your IMEI (15 digits)" />
        <select id="brandSelect">
            <option value="samsung">Samsung</option>
            <option value="apple">Apple</option>
            <option value="xiaomi">Xiaomi</option>
        </select>
        <button onclick="checkIMEI()">Check IMEI</button>

        <!-- CAPTCHA Section (for bots prevention) -->
        <div id="captcha">
            <input type="checkbox" id="captchaCheck"> I'm not a robot
        </div>
        
        <div id="result" class="result"></div>
    </div>

    <div class="brand-section">
        <h2>Popular Firmware Updates</h2>
        <ul>
            <li><a href="/samsung">Samsung Firmware</a></li>
            <li><a href="/apple">Apple Firmware</a></li>
            <li><a href="/xiaomi">Xiaomi Firmware</a></li>
        </ul>
    </div>

    <script>
        function isValidIMEI(imei) {
            if (imei.length !== 15 || isNaN(imei)) {
                return false;
            }
            let sum = 0;
            for (let i = 0; i < imei.length; i++) {
                let num = parseInt(imei[i]);
                if (i % 2 === 1) {
                    num *= 2;
                    if (num > 9) num -= 9;
                }
                sum += num;
            }
            return sum % 10 === 0;
        }

        function checkIMEI() {
            const imei = document.getElementById("imei").value.trim();
            const brand = document.getElementById("brandSelect").value;
            const captchaChecked = document.getElementById("captchaCheck").checked;
            const resultDiv = document.getElementById("result");

            if (!isValidIMEI(imei)) {
                resultDiv.innerHTML = "<p style='color: red;'>Invalid IMEI format.</p>";
                return;
            }
            if (!captchaChecked) {
                resultDiv.innerHTML = "<p style='color: red;'>Please verify you're not a robot.</p>";
                return;
            }

            // Simulated database lookup based on brand
            const database = {
                samsung: {
                    "123456789012345": {
                        model: "Samsung Galaxy S23",
                        country: "USA",
                        carrier: "Verizon",
                    },
                    "356789012345678": {
                        model: "Samsung Galaxy S21",
                        country: "Germany",
                        carrier: "T-Mobile",
                    }
                },
                apple: {
                    "098765432109876": {
                        model: "iPhone 13 Pro",
                        country: "USA",
                        carrier: "AT&T",
                    }
                },
                xiaomi: {
                    "135792468013579": {
                        model: "Xiaomi Mi 11",
                        country: "China",
                        carrier: "China Mobile",
                    }
                }
            };

            const imeiData = database[brand] && database[brand][imei];
            if (imeiData) {
                resultDiv.innerHTML = `
                    <p style='color: green;'>IMEI Found:</p>
                    <p><strong>Model:</strong> ${imeiData.model}</p>
                    <p><strong>Country:</strong> ${imeiData.country}</p>
                    <p><strong>Carrier:</strong> ${imeiData.carrier}</p>
                `;
            } else {
                resultDiv.innerHTML = "<p style='color: red;'>IMEI not found in database.</p>";
            }
        }
    </script>
</body>
</html>