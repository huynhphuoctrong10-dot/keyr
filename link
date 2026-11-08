<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hệ Thống Vượt Link Tự Động</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f6f9;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .box {
            background: #ffffff;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 450px;
            text-align: center;
        }
        h2 {
            margin-bottom: 15px;
            color: #333;
            font-size: 20px;
        }
        input, textarea {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 15px;
            outline: none;
        }
        input:focus {
            border-color: #007bff;
        }
        button {
            width: 100%;
            padding: 12px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.3s;
        }
        button:hover {
            background-color: #0056b3;
        }
        .youtube-btn {
            display: block;
            width: 100%;
            background-color: #ff0000;
            color: white;
            padding: 14px;
            border-radius: 6px;
            text-decoration: none;
            font-weight: bold;
            font-size: 16px;
            box-sizing: border-box;
            margin-bottom: 20px;
            box-shadow: 0 4px 10px rgba(255, 0, 0, 0.2);
        }
        .youtube-btn:hover {
            background-color: #cc0000;
        }
        .submit-btn {
            background-color: #28a745;
        }
        .submit-btn:hover {
            background-color: #218838;
        }
        .error {
            color: #d9534f;
            margin-top: 12px;
            font-size: 14px;
            font-weight: bold;
        }
        .note {
            color: #555;
            font-size: 13px;
            margin-bottom: 20px;
            text-align: left;
            background: #fff8f8;
            padding: 10px;
            border-left: 4px solid #d9534f;
            border-radius: 4px;
        }
        .instruction {
            color: #d9534f;
            font-weight: bold;
            font-size: 14px;
            margin-bottom: 10px;
        }
        .result-box {
            background: #e9ecef;
            padding: 15px;
            border-radius: 6px;
            word-break: break-all;
            margin-top: 15px;
            display: none;
        }
    </style>
</head>
<body>

    <div class="box" id="app">
        <!-- Nội dung sẽ tự thay đổi tùy theo cách bạn truy cập -->
    </div>

    <script>
        const urlParams = new URLSearchParams(window.location.search);
        const encodedData = urlParams.get('d');
        const appDiv = document.getElementById('app');

        if (!encodedData) {
            // ================= TRANG TẠO LINK (Dành cho bạn) =================
            appDiv.innerHTML = `
                <h2>Tạo Link Vượt Nhanh</h2>
                <p style="font-size:13px; color:#666; margin-bottom:15px;">Điền thông tin bên dưới để tạo ngay 1 link vượt pass riêng:</p>
                
                <input type="text" id="passInput" placeholder="Nhập Mật Khẩu (Ví dụ: tmg009)">
                <input type="text" id="targetInput" placeholder="Nhập Link MediaFire đích">
                <input type="text" id="videoInput" placeholder="Nhập Link Video YouTube">
                
                <button onclick="generateLink()">Tạo Link Ngay</button>
                
                <div id="resultContainer" class="result-box">
                    <b>Link của bạn đây (Copy lại nhé):</b><br>
                    <input type="text" id="finalLink" readonly style="margin-top:8px; background:#fff; text-align:center;">
                    <button onclick="copyLink()" style="background:#17a2b8; margin-top:5px;">Sao chép Link</button>
                </div>
            `;
        } else {
            // ================= TRANG VƯỢT LINK (Dành cho người xem) =================
            try {
                const decodedStr = atob(encodedData);
                const data = JSON.parse(decodedStr);

                appDiv.innerHTML = `
                    <h2>Xác Thực Vượt Link</h2>
                    
                    <a href="${data.v}" target="_blank" class="youtube-btn">
                        ▶ Bấm vào đây để xem video lấy Pass
                    </a>

                    <div class="instruction">Hãy Xem Hết Video để lấy pass và nhập pass vào đây!</div>
                    
                    <div class="note">
                        <strong>💡 Gợi ý:</strong> Pass là phần chữ màu trắng trong video, tới lúc pass chuyển sang màu đỏ sẽ hiện pass.
                    </div>
                    
                    <input type="password" id="passwordInput" placeholder="Nhập mật khẩu vào đây..." autofocus>
                    <button class="submit-btn" onclick="verifyPassword('${data.p}', '${data.t}')">Lấy Link Tải</button>
                    
                    <div id="errorMessage" class="error"></div>
                `;
            } catch (e) {
                appDiv.innerHTML = `<h2>Lỗi</h2><p style="color:red;">Link không hợp lệ hoặc đã bị hỏng!</p>`;
            }
        }

        function generateLink() {
            const p = document.getElementById('passInput').value.trim();
            const t = document.getElementById('targetInput').value.trim();
            const v = document.getElementById('videoInput').value.trim();

            if (!p || !t || !v) {
                alert('Vui lòng điền đầy đủ tất cả các ô!');
                return;
            }

            const rawData = JSON.stringify({ p, t, v });
            const b64 = btoa(unescape(encodeURIComponent(rawData))); 
            const fullUrl = window.location.origin + window.location.pathname + '?d=' + b64;

            document.getElementById('finalLink').value = fullUrl;
            document.getElementById('resultContainer').style.display = 'block';
        }

        function copyLink() {
            const copyText = document.getElementById("finalLink");
            copyText.select();
            copyText.setSelectionRange(0, 99999);
            navigator.clipboard.writeText(copyText.value);
            alert("Đã sao chép link thành công!");
        }

        function verifyPassword(correctPass, targetUrl) {
            const inputVal = document.getElementById("passwordInput").value.trim();
            const errorDiv = document.getElementById("errorMessage");

            if (inputVal === correctPass) {
                errorDiv.style.color = "green";
                errorDiv.innerText = "Chính xác! Đang chuyển hướng đến MediaFire...";
                setTimeout(() => {
                    window.location.href = targetUrl;
                }, 1000);
            } else {
                errorDiv.style.color = "#d9534f";
                errorDiv.innerText = "Mật khẩu không chính xác. Hãy xem kỹ video nhé!";
                document.getElementById("passwordInput").value = "";
                document.getElementById("passwordInput").focus();
            }
        }

        document.addEventListener("keypress", function(event) {
            if (event.key === "Enter" && document.getElementById("passwordInput")) {
                const urlParams = new URLSearchParams(window.location.search);
                const decodedStr = atob(urlParams.get('d'));
                const data = JSON.parse(decodedStr);
                verifyPassword(data.p, data.t);
            }
        });
    </script>

</body>
</html>
