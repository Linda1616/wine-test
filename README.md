<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>紅酒 RWA 防偽追蹤系統</title>
    <style>
        body { font-family: 'PingFang TC', sans-serif; background: #fdfcfb; color: #333; text-align: center; padding: 20px; }
        .container { max-width: 450px; margin: 0 auto; border: 1px solid #e0e0e0; border-radius: 20px; padding: 30px; background: white; box-shadow: 0 10px 30px rgba(0,0,0,0.05); }
        .wine-tag { font-size: 12px; color: #800020; border: 1px solid #800020; padding: 2px 8px; border-radius: 10px; font-weight: bold; }
        .status-box { margin: 20px 0; padding: 15px; border-radius: 10px; font-weight: bold; transition: all 0.5s; }
        .shipping { background: #ebf8ff; color: #2b6cb0; border: 1px dashed #2b6cb0; }
        .redeemed { background: #fff5f5; color: #c53030; border: 1px solid #c53030; }
        .btn { background: #800020; color: white; border: none; padding: 15px 20px; border-radius: 8px; width: 100%; font-size: 16px; cursor: pointer; font-weight: bold; }
        .btn:disabled { background: #ccc; cursor: not-allowed; }
        .info-panel { text-align: left; font-size: 13px; color: #666; background: #f9f9f9; padding: 15px; border-radius: 10px; margin-top: 20px; line-height: 1.6; }
        code { background: #eee; padding: 2px 5px; border-radius: 4px; font-family: monospace; }
    </style>
</head>
<body>
    <div class="container">
        <span class="wine-tag">RWA Anti-Counterfeit System</span>
        <h2 style="margin-top: 15px;">🍷 紅酒資產驗證</h2>
        <hr style="border: 0; border-top: 1px solid #eee;">
        
        <p>資產代碼 Token ID: <strong id="tid">載入中...</strong></p>
        <p style="font-size: 14px;">物理識別碼 UID: <code id="uid">讀取中...</code></p>
        
        <div id="statusArea" class="status-box shipping">
            🚚 物流狀態：運輸中 (封條完整)
        </div>

        <div class="info-panel">
            <strong>🛡️ 防偽技術說明：</strong><br>
            1. <strong>實體綁定：</strong>此標籤 UID 為 NTAG213 硬體唯一碼，不可複製。<br>
            2. <strong>防替換機制：</strong>若外盒封條被撕毀，天線將斷裂導致無法感應。<br>
            3. <strong>數位存證：</strong>點擊收貨後，合約狀態將改為 <code>isRedeemed = true</code>，此過程不可逆，防止二次銷售。
        </div>

        <p id="msg" style="font-size: 12px; color: #800020; min-height: 18px; margin: 15px 0;"></p>
        
        <button class="btn" id="actionBtn" onclick="confirmRedeem()">確認收貨（銷毀數位封條）</button>
    </div>

    <script>
        // 抓取網址參數
        const urlParams = new URLSearchParams(window.location.search);
        const tokenID = urlParams.get('id') || '1';
        document.getElementById('tid').innerText = tokenID;
        
        // 模擬讀取 NFC 硬體 UID
        document.getElementById('uid').innerText = 'NTAG213-BE42' + tokenID + 'A8';

        function confirmRedeem() {
            const btn = document.getElementById('actionBtn');
            const status = document.getElementById('statusArea');
            const msg = document.getElementById('msg');

            btn.disabled = true;
            msg.innerText = "⏳ 正在將「已拆封」狀態寫入 Remix VM 合約...";
            
            setTimeout(() => {
                status.className = "status-box redeemed";
                status.innerHTML = "🚫 狀態：已簽收 (數位封條已銷毀)";
                msg.innerText = "✅ 區塊鏈同步成功：該資產已完成最終交付。";
                
                // 模擬合約回饋
                console.log("Contract state updated: isRedeemed = true for Token " + tokenID);
            }, 1800);
        }
    </script>
</body>
</html>
