<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI GPT Trade Bot Pro</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        :root {
            --bg-color: #0b0f19;
            --card-bg: #131b2e;
            --accent-green: #0ecb81;
            --accent-red: #f6465d;
            --text-main: #f0f6fc;
            --text-muted: #8b949e;
            --border-color: #21262d;
            --gold: #f0b90b;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            padding: 16px;
            padding-bottom: 90px;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
        }

        .brand {
            font-size: 16px;
            font-weight: 800;
            color: #ffffff;
            display: flex;
            align-items: center;
            gap: 6px;
            letter-spacing: 0.5px;
        }

        .live-status {
            font-size: 11px;
            color: var(--accent-green);
            background: rgba(14, 203, 129, 0.1);
            padding: 4px 8px;
            border-radius: 20px;
            display: flex;
            align-items: center;
            gap: 5px;
            font-weight: 600;
        }

        .live-dot {
            width: 6px;
            height: 6px;
            background-color: var(--accent-green);
            border-radius: 50%;
            box-shadow: 0 0 8px var(--accent-green);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.3; }
            100% { opacity: 1; }
        }

        .trial-banner {
            background: linear-gradient(135deg, #18223c 0%, #111827 100%);
            border: 1px solid rgba(240, 185, 11, 0.3);
            border-radius: 14px;
            padding: 14px;
            text-align: center;
            margin-bottom: 16px;
        }

        .trial-title {
            color: var(--gold);
            font-size: 13px;
            font-weight: 700;
            margin-bottom: 6px;
            letter-spacing: 0.5px;
        }

        .trial-dots {
            display: flex;
            justify-content: center;
            gap: 6px;
        }

        .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background-color: var(--gold);
        }

        .dot.empty {
            background-color: var(--border-color);
        }

        .greeting {
            font-size: 20px;
            font-weight: 700;
            color: #ffffff;
            margin-bottom: 2px;
        }

        .subtitle {
            font-size: 12px;
            color: var(--text-muted);
            margin-bottom: 16px;
        }

        .card {
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 14px;
            padding: 16px;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .card:active {
            transform: scale(0.97);
            border-color: var(--gold);
        }

        .card-left h3 {
            font-size: 15px;
            color: #ffffff;
            margin-bottom: 4px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .card-left p {
            font-size: 11px;
            color: var(--text-muted);
        }

        .badge-new {
            background: linear-gradient(135deg, #8957e5 0%, #6e40c9 100%);
            color: #ffffff;
            font-size: 9px;
            padding: 2px 6px;
            border-radius: 4px;
            font-weight: 700;
            text-transform: uppercase;
        }

        .arrow-icon {
            font-size: 16px;
            color: var(--text-muted);
        }

        .setups-heading {
            font-size: 13px;
            font-weight: 700;
            color: var(--text-main);
            margin: 20px 0 10px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
            letter-spacing: 0.5px;
        }

        .setup-item {
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 12px 14px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .setup-info h4 {
            font-size: 14px;
            color: #ffffff;
            margin-bottom: 2px;
            font-weight: 600;
        }

        .setup-info span {
            font-size: 11px;
            font-weight: 600;
        }

        .put-text { color: var(--accent-red); }
        .call-text { color: var(--accent-green); }

        .setup-right {
            text-align: right;
        }

        .confidence {
            font-size: 14px;
            font-weight: 700;
            color: var(--accent-green);
        }

        .conf-label {
            font-size: 9px;
            color: var(--text-muted);
            letter-spacing: 0.5px;
        }

        /* Modal Overlay for AI Generating effect */
        #loadingModal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(11, 15, 25, 0.9);
            z-index: 999;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            text-align: center;
        }

        .spinner {
            width: 45px;
            height: 45px;
            border: 4px solid var(--border-color);
            border-top: 4px solid var(--gold);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
            margin-bottom: 12px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>

    <div class="header">
        <div class="brand">⚡ AI GPT TRADE BOT</div>
        <div class="live-status">
            <div class="live-dot"></div> AI ENGINE ACTIVE
        </div>
    </div>

    <div class="trial-banner">
        <div class="trial-title">⚡ VIP FREE TRIAL ACTIVE ⚡</div>
        <div class="trial-dots">
            <div class="dot"></div>
            <div class="dot empty"></div>
        </div>
        <div style="font-size: 10px; color: var(--text-muted); margin-top: 4px;">0/2 free signals used today</div>
    </div>

    <div class="greeting">Hey, Trader 👋</div>
    <div class="subtitle">Select your strategy to get high-accuracy signals</div>

    <div class="card" onclick="triggerAIAction('Photo Analysis')">
        <div class="card-left">
            <h3>📷 Photo Analysis <span class="badge-new">PRO</span></h3>
            <p>Upload chart screenshot for instant AI signal breakdown</p>
        </div>
        <div class="arrow-icon">➔</div>
    </div>

    <div class="card" onclick="triggerAIAction('Manual Signal Generator')">
        <div class="card-left">
            <h3>⚡ Manual Signal Generator</h3>
            <p>Select currency pair & timeframe for precise market direction</p>
        </div>
        <div class="arrow-icon">➔</div>
    </div>

    <div class="setups-heading">
        <span>🔥 LIVE MARKET SETUPS</span>
        <span style="font-size: 10px; color: var(--text-muted);">Real-time feed</span>
    </div>

    <div class="setup-item">
        <div class="setup-info">
            <h4>XAU/USD (Gold)</h4>
            <span class="put-text">🔻 PUT Signal</span>
        </div>
        <div class="setup-right">
            <div class="confidence">96.8%</div>
            <div class="conf-label">ACCURACY</div>
        </div>
    </div>

    <div class="setup-item">
        <div class="setup-info">
            <h4>EUR/USD OTC</h4>
            <span class="call-text">🟢 CALL Signal</span>
        </div>
        <div class="setup-right">
            <div class="confidence">93.2%</div>
            <div class="conf-label">ACCURACY</div>
        </div>
    </div>

    <div class="setup-item">
        <div class="setup-info">
            <h4>BTC/USD</h4>
            <span class="call-text">🟢 CALL Signal</span>
        </div>
        <div class="setup-right">
            <div class="confidence">98.1%</div>
            <div class="conf-label">ACCURACY</div>
        </div>
    </div>

    <div id="loadingModal">
        <div class="spinner"></div>
        <div style="font-size: 14px; font-weight: 600; color: #ffffff;" id="loadingText">AI Analyzing Market...</div>
        <div style="font-size: 11px; color: var(--text-muted); margin-top: 4px;">Connecting to neural network</div>
    </div>

    <script>
        // Telegram WebApp Integration
        let tg = window.Telegram.WebApp;
        tg.expand();

        function triggerAIAction(actionName) {
            // Haptic vibration feedback if supported
            if (tg.HapticFeedback) {
                tg.HapticFeedback.impactOccurred('medium');
            }

            const modal = document.getElementById('loadingModal');
            const text = document.getElementById('loadingText');
            modal.style.display = 'flex';
            text.innerText = `Initializing ${actionName}...`;

            setTimeout(() => {
                modal.style.display = 'none';
                alert(`🚀 ${actionName} opened successfully! Connect your trading interface to receive live signals.`);
            }, 1200);
        }
    </script>
</body>
</html>
