<?php
// ==========================================
// 1. CẤU HÌNH BẢO MẬT HỆ THỐNG
// ==========================================
$ALLOWED_DOMAIN = "tentaikhoan.github.io"; // Thay bằng tên miền website GitHub của bạn (không gồm https://)
$SECRET_SALT = "Trong_Dep_Trai_Secure_2026"; // Chuỗi bí mật ngầm

// 2. KIỂM TRA TÊN MIỀN TRUY CẬP (REFERER CHECK)
$referer = isset($_SERVER['HTTP_REFERER']) ? $_SERVER['HTTP_REFERER'] : '';
$referer_host = parse_url($referer, PHP_URL_HOST);

if (empty($referer_host) || strpos($referer_host, $ALLOWED_DOMAIN) === false) {
    header('HTTP/1.0 403 Forbidden');
    echo "<div style='text-align:center; margin-top:50px; font-family:Arial;'>
            <h2 style='color:red;'>⚠️ TRUY CẬP BỊ TỪ CHỐI!</h2>
            <p>Bạn phải truy cập đúng từ website chính thức để lấy Key.</p>
          </div>";
    exit();
}

// 3. TẠO MÃ KEY TỰ ĐỘNG ĐỔI THEO NGÀY
$today = date("Ymd");
$raw_hash = md5($today . "_" . $SECRET_SALT);
$short_code = strtoupper(substr($raw_hash, 0, 4));
$CURRENT_KEY = "L4M_" . date("dm") . "_" . $short_code;
?>

<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Xác Nhận Lấy Key</title>
    <style>
        * { box-sizing: border-box; font-family: Arial, sans-serif; }
        body { background: #f0f2f5; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; padding: 15px; }
        .card { background: #fff; padding: 25px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.15); text-align: center; max-width: 400px; width: 100%; }
        .timer-box { font-size: 14px; color: #d9534f; font-weight: bold; margin-bottom: 10px; background: #fff0f0; padding: 8px; border-radius: 5px; }
        .key-box { font-size: 22px; font-weight: bold; color: #28a745; background: #e8f5e9; padding: 12px; border-radius: 8px; border: 2px dashed #28a745; margin: 15px 0; letter-spacing: 2px; word-break: break-all; }
        .btn { background: #007bff; color: white; padding: 10px 18px; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; font-size: 15px; }
        .btn:hover { background: #0056b3; }
        .btn:disabled { background: #ccc; cursor: not-allowed; }
        .expired-msg { color: red; font-weight: bold; display: none; }
    </style>
</head>
<body>

    <div class="card">
        <h3>🎉 Vượt Link Thành Công!</h3>
        
        <div class="timer-box" id="timer-display">⏱️ Key sẽ biến mất sau: <span id="countdown">30</span> giây</div>

        <div class="key-box" id="key-text"><?php echo $CURRENT_KEY; ?></div>
        
        <button class="btn" id="copy-btn" onclick="copyAndDestroyKey()">📋 Sao Chép Key</button>

        <p class="expired-msg" id="expired-text">⚠️ Key đã bị xóa hoặc hết thời gian! Vui lòng vượt link lại để lấy Key mới.</p>
    </div>

    <script>
        let timeLeft = 30;
        let isCopied = false;

        // Đếm ngược 30s
        const timerInterval = setInterval(function() {
            timeLeft--;
            document.getElementById("countdown").innerText = timeLeft;

            if (timeLeft <= 0) {
                destroyKey("Hết thời gian 30s!");
            }
        }, 1000);

        // Hàm sao chép và tự động xóa Key ngay lập tức
        function copyAndDestroyKey() {
            if (isCopied || timeLeft <= 0) return;

            let keyText = document.getElementById("key-text").innerText;
            navigator.clipboard.writeText(keyText);

            alert("Đã sao chép Key: " + keyText + "\n(Key sẽ bị xóa ngay lập tức!)");
            
            isCopied = true;
            destroyKey("Đã sao chép 1 lần!");
        }

        // Hàm tiêu hủy Key khỏi giao diện
        function destroyKey(reason) {
            clearInterval(timerInterval);
            document.getElementById("key-text").style.display = "none";
            document.getElementById("copy-btn").style.display = "none";
            document.getElementById("timer-display").style.display = "none";
            
            let expiredEl = document.getElementById("expired-text");
            expiredEl.style.display = "block";
        }
    </script>

</body>
</html>
