<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>硅基流动 · 用户信息及余额查询</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            font-family: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(145deg, #0b1a2e 0%, #1b2f45 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 1rem;
        }

        .card {
            background: rgba(18, 28, 40, 0.9);
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            border: 1px solid rgba(66, 153, 225, 0.3);
            border-radius: 2rem;
            box-shadow: 0 25px 40px -10px rgba(0, 0, 0, 0.6), 0 0 0 1px rgba(66, 153, 225, 0.2) inset;
            width: 100%;
            max-width: 700px;
            padding: 2rem 2rem 2.5rem;
            color: #e3e9f2;
            transition: all 0.2s;
        }

        h2 {
            font-size: 1.9rem;
            font-weight: 600;
            letter-spacing: -0.01em;
            margin: 0 0 0.25rem 0;
            background: linear-gradient(120deg, #a0d2ff, #6cb2eb);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        h2 span {
            font-size: 2rem;
        }

        .sub {
            color: #8fa6c0;
            margin-bottom: 2rem;
            font-size: 0.9rem;
            border-left: 3px solid #2b4f6e;
            padding-left: 1rem;
        }

        .input-group {
            margin-bottom: 1.5rem;
        }

        label {
            display: block;
            font-size: 0.85rem;
            font-weight: 500;
            text-transform: uppercase;
            letter-spacing: 0.02em;
            color: #9bb5d4;
            margin-bottom: 0.5rem;
        }

        .key-wrapper {
            display: flex;
            align-items: center;
            background: #0f1a24;
            border: 1px solid #2a4055;
            border-radius: 60px;
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        .key-wrapper:focus-within {
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59,130,246,0.2);
        }

        .key-wrapper input {
            flex: 1;
            background: transparent;
            border: none;
            padding: 0.9rem 1.2rem;
            font-size: 1rem;
            color: #f0f6ff;
            outline: none;
        }

        .key-wrapper input::placeholder {
            color: #3f5568;
            font-weight: 300;
        }

        .show-check {
            display: flex;
            align-items: center;
            gap: 6px;
            padding-right: 1rem;
            color: #a2c0da;
            font-size: 0.9rem;
            cursor: pointer;
            user-select: none;
        }
        .show-check input {
            width: 18px;
            height: 18px;
            accent-color: #3b82f6;
            margin: 0;
        }

        .endpoint-wrapper {
            display: flex;
            background: #0f1a24;
            border: 1px solid #2a4055;
            border-radius: 60px;
            overflow: hidden;
        }
        .endpoint-wrapper input {
            flex: 1;
            background: transparent;
            border: none;
            padding: 0.9rem 1.2rem;
            font-size: 0.95rem;
            color: #d6e4ff;
            outline: none;
        }
        .endpoint-wrapper input:focus {
            background: #121f2c;
        }

        button {
            width: 100%;
            background: linear-gradient(145deg, #2563eb, #1d4ed8);
            border: none;
            padding: 1rem 1.5rem;
            border-radius: 60px;
            font-size: 1.2rem;
            font-weight: 600;
            color: white;
            letter-spacing: 0.3px;
            cursor: pointer;
            box-shadow: 0 10px 20px -8px #0f2b50;
            transition: transform 0.1s, box-shadow 0.2s, background 0.2s;
            margin: 1.5rem 0 1.8rem;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 12px;
        }

        button:active {
            transform: scale(0.98);
            box-shadow: 0 5px 12px -5px #0b1e33;
        }

        button:disabled {
            opacity: 0.65;
            background: #3a4c66;
            box-shadow: none;
            cursor: not-allowed;
            transform: none;
        }

        .result {
            background: #0d1722;
            border-radius: 28px;
            padding: 1.5rem;
            border: 1px solid #293e54;
            word-wrap: break-word;
        }

        .summary {
            background: #162330;
            border-radius: 20px;
            padding: 1.2rem 1.5rem;
            margin-bottom: 1.5rem;
            border-left: 5px solid #3b82f6;
            font-size: 1.2rem;
            display: flex;
            flex-wrap: wrap;
            align-items: baseline;
            gap: 1rem 2rem;
        }

        .summary-item {
            display: flex;
            align-items: baseline;
            gap: 8px;
        }

        .summary-item .tag {
            font-size: 0.85rem;
            color: #8aa9cc;
            text-transform: uppercase;
            font-weight: 500;
        }

        .summary-item .value {
            font-size: 1.6rem;
            font-weight: 600;
            color: #ecf5ff;
            letter-spacing: -0.02em;
        }

        .summary-item .unit {
            font-size: 0.9rem;
            color: #7c9bc0;
            margin-left: 4px;
        }

        .raw-title {
            font-size: 0.8rem;
            text-transform: uppercase;
            color: #5f7d9c;
            letter-spacing: 0.5px;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .raw-json {
            background: #0b131e;
            padding: 1.2rem;
            border-radius: 18px;
            border: 1px solid #253a4b;
            font-family: 'SF Mono', 'Fira Code', monospace;
            font-size: 0.85rem;
            line-height: 1.6;
            color: #bed9ff;
            overflow-x: auto;
            white-space: pre-wrap;
            word-break: break-word;
            max-height: 250px;
            overflow-y: auto;
        }

        .error-message {
            background: rgba(200, 70, 70, 0.15);
            border-left: 4px solid #e54c4c;
            padding: 1rem 1.3rem;
            border-radius: 20px;
            color: #ffb3b3;
            font-weight: 400;
        }

        .footer-note {
            margin-top: 1.5rem;
            font-size: 0.8rem;
            text-align: center;
            color: #5f7d9c;
            border-top: 1px dashed #2a4055;
            padding-top: 1rem;
        }

        .footer-note a {
            color: #8ab9ff;
            text-decoration: none;
        }

        .spinner {
            display: inline-block;
            width: 18px;
            height: 18px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 0.8s linear infinite;
        }
        @keyframes spin { to { transform: rotate(360deg); } }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
<div class="card">
    <h2>
        <span>👤</span> 硅基流动 · 用户信息与余额
    </h2>
    <div class="sub">
        基于 <code style="background:#1e2a36; padding:2px 6px; border-radius:30px;">/v1/user/info</code> 接口查询，密钥仅存于本地
    </div>

    <!-- API密钥输入区（带显示切换） -->
    <div class="input-group">
        <label>🔑 API 密钥</label>
        <div class="key-wrapper">
            <input type="password" id="apiKeyInput" placeholder="sk- 或 sif- 开头密钥" autocomplete="off" spellcheck="false">
            <label class="show-check">
                <input type="checkbox" id="showKeyCheck"> 显示
            </label>
        </div>
    </div>

    <!-- API地址输入（已预设为您找到的正确接口） -->
    <div class="input-group">
        <label>🌐 API 地址 (已预设为 /user/info)</label>
        <div class="endpoint-wrapper">
            <input type="url" id="endpointInput" value="https://api.siliconflow.cn/v1/user/info" placeholder="https://api.siliconflow.cn/v1/user/info">
        </div>
        <div style="font-size:0.75rem; color:#5f7d9c; margin-top:6px; padding-left:14px;">
            ⚡ 这是您提供的正确接口，如需修改可手动编辑
        </div>
    </div>

    <!-- 查询按钮 -->
    <button id="queryBtn">
        <span>🔎</span> 查询余额
        <span class="spinner hidden" id="btnSpinner"></span>
    </button>

    <!-- 结果显示区 -->
    <div class="result" id="resultArea">
        <div class="summary" id="balanceSummary">
            <div class="summary-item">
                <span class="tag">余额</span>
                <span class="value" id="displayBalance">—</span>
            </div>
            <div class="summary-item">
                <span class="tag">总余额</span>
                <span class="value" id="displayTotal">—</span>
            </div>
        </div>
        <div class="raw-title">
            <span>📋 完整响应 (JSON)</span>
        </div>
        <pre class="raw-json" id="rawJson">等待查询…</pre>
        <div id="errorBox" class="error-message hidden"></div>
    </div>

    <div class="footer-note">
        🔐 密钥仅存于当前页面，不会发送到任何第三方 · 查询直接调用您填入的API地址
    </div>
</div>

<script>
    (function() {
        // DOM elements
        const apiKeyInput = document.getElementById('apiKeyInput');
        const showKeyCheck = document.getElementById('showKeyCheck');
        const endpointInput = document.getElementById('endpointInput');
        const queryBtn = document.getElementById('queryBtn');
        const btnSpinner = document.getElementById('btnSpinner');
        const displayBalance = document.getElementById('displayBalance');
        const displayTotal = document.getElementById('displayTotal');
        const rawJsonPre = document.getElementById('rawJson');
        const errorBox = document.getElementById('errorBox');
        const balanceSummary = document.getElementById('balanceSummary');

        // 辅助函数：隐藏/显示错误
        function showError(msg) {
            errorBox.classList.remove('hidden');
            errorBox.innerText = msg || '发生未知错误';
            rawJsonPre.innerText = '⚠️ ' + msg;
        }

        function hideError() {
            errorBox.classList.add('hidden');
        }

        function setLoading(isLoading) {
            if (isLoading) {
                queryBtn.disabled = true;
                btnSpinner.classList.remove('hidden');
                queryBtn.innerHTML = `<span>⏳</span> 查询中... <span class="spinner"></span>`;
            } else {
                queryBtn.disabled = false;
                btnSpinner.classList.add('hidden');
                queryBtn.innerHTML = `<span>🔎</span> 查询余额 <span class="spinner hidden"></span>`;
            }
        }

        // 显示密钥明文/密文
        showKeyCheck.addEventListener('change', function(e) {
            apiKeyInput.type = this.checked ? 'text' : 'password';
        });

        // 针对 /user/info 接口提取常见的余额字段
        function extractBalanceInfo(json) {
            let balance = null;
            let total = null;

            // 直接尝试常见的字段名 (根据 /user/info 可能的结构)
            if (json.balance !== undefined) balance = json.balance;
            else if (json.credit !== undefined) balance = json.credit;
            else if (json.amount !== undefined) balance = json.amount;
            else if (json.data?.balance !== undefined) balance = json.data.balance;
            else if (json.data?.credit !== undefined) balance = json.data.credit;

            if (json.totalBalance !== undefined) total = json.totalBalance;
            else if (json.total_balance !== undefined) total = json.total_balance;
            else if (json.totalAmount !== undefined) total = json.totalAmount;
            else if (json.data?.totalBalance !== undefined) total = json.data.totalBalance;
            else if (json.data?.total_balance !== undefined) total = json.data.total_balance;

            // 如果还是没有，尝试查看是否有嵌套的 user 对象（例如：{ "user": { "balance": 100 } }）
            if (balance === null && json.user && typeof json.user === 'object') {
                if (json.user.balance !== undefined) balance = json.user.balance;
                else if (json.user.credit !== undefined) balance = json.user.credit;
            }
            if (total === null && json.user && typeof json.user === 'object') {
                if (json.user.totalBalance !== undefined) total = json.user.totalBalance;
                else if (json.user.total_balance !== undefined) total = json.user.total_balance;
            }

            return { balance, total };
        }

        // 更新UI: 展示余额和原始json
        function displayResult(data) {
            hideError();

            // 格式化json展示
            let prettyJson = '';
            try {
                prettyJson = JSON.stringify(data, null, 2);
            } catch (e) {
                prettyJson = '[无法序列化数据]';
            }
            rawJsonPre.innerText = prettyJson;

            // 提取余额摘要
            const { balance, total } = extractBalanceInfo(data);

            // 更新摘要区域
            displayBalance.innerText = (balance !== null && balance !== undefined) ? balance : '—';
            displayTotal.innerText = (total !== null && total !== undefined) ? total : '—';
        }

        // 重置显示（查询前可调用，但非必须）
        function resetDisplay() {
            displayBalance.innerText = '—';
            displayTotal.innerText = '—';
            rawJsonPre.innerText = '查询中，请稍后……';
            hideError();
        }

        // 主要查询逻辑
        async function performQuery() {
            const apiKey = apiKeyInput.value.trim();
            if (!apiKey) {
                showError('请输入 API 密钥');
                return;
            }

            let endpoint = endpointInput.value.trim();
            if (!endpoint) {
                showError('API 地址不能为空');
                return;
            }

            // 简单的URL合法性校验
            try {
                new URL(endpoint);
            } catch (err) {
                showError('API 地址格式不正确，请输入完整的URL (包含https://)');
                return;
            }

            resetDisplay();
            setLoading(true);

            try {
                const controller = new AbortController();
                const timeoutId = setTimeout(() => controller.abort(), 10000);

                const response = await fetch(endpoint, {
                    method: 'GET',
                    headers: {
                        'Authorization': `Bearer ${apiKey}`,
                        'Accept': 'application/json',
                    },
                    signal: controller.signal,
                    mode: 'cors',
                });

                clearTimeout(timeoutId);

                let responseData;
                const contentType = response.headers.get('content-type') || '';
                if (contentType.includes('application/json')) {
                    responseData = await response.json();
                } else {
                    const text = await response.text();
                    responseData = { raw_text: text, status: response.status };
                }

                if (!response.ok) {
                    let errorMsg = `HTTP ${response.status}`;
                    if (responseData && responseData.message) errorMsg += `: ${responseData.message}`;
                    else if (responseData && responseData.error) errorMsg += `: ${responseData.error}`;
                    else if (responseData && responseData.msg) errorMsg += `: ${responseData.msg}`;
                    else if (responseData && responseData.detail) errorMsg += `: ${responseData.detail}`;
                    else if (responseData && responseData.raw_text) errorMsg += ` - ${responseData.raw_text.substring(0,100)}`;
                    
                    // 特别处理401错误，给出更友好的提示
                    if (response.status === 401) {
                        errorMsg = '认证失败 (401)，请检查 API 密钥是否有效';
                    }
                    throw new Error(errorMsg);
                }

                displayResult(responseData);

            } catch (error) {
                console.warn('查询异常:', error);

                if (error.name === 'AbortError') {
                    showError('请求超时，请稍后重试或检查网络/API地址');
                    rawJsonPre.innerText = '请求超时，未收到响应';
                } else if (error.message.includes('Failed to fetch') || error.message.includes('NetworkError')) {
                    showError('网络错误 / CORS 跨域问题。可尝试使用浏览器插件或自建代理。');
                    rawJsonPre.innerText = '网络请求失败。如果是跨域问题，可尝试通过CORS代理。';
                } else {
                    showError(error.message || '未知错误');
                }

                displayBalance.innerText = '—';
                displayTotal.innerText = '—';
            } finally {
                setLoading(false);
            }
        }

        // 绑定事件
        queryBtn.addEventListener('click', performQuery);

        apiKeyInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault();
                queryBtn.click();
            }
        });
        endpointInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault();
                queryBtn.click();
            }
        });

        apiKeyInput.focus();
    })();
</script>
</body>
</html>
