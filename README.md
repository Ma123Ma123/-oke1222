[weep 1.html](https://github.com/user-attachments/files/24536393/weep.1.html)
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>محاكي Terminal - واجهة أوامر تفاعلية</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Courier New', monospace;
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a2e 100%);
            color: #00ff00;
            height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            width: 90%;
            max-width: 1200px;
            height: 85vh;
            background: rgba(0, 0, 0, 0.95);
            border-radius: 10px;
            box-shadow: 0 0 50px rgba(0, 255, 0, 0.3);
            border: 2px solid #00ff00;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(90deg, #0f3460 0%, #16213e 100%);
            padding: 10px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 2px solid #00ff00;
        }

        .window-controls {
            display: flex;
            gap: 8px;
        }

        .control-btn {
            width: 15px;
            height: 15px;
            border-radius: 50%;
            border: none;
            cursor: pointer;
        }

        .close { background: #ff5f56; }
        .minimize { background: #ffbd2e; }
        .maximize { background: #27c93f; }

        .title {
            color: #00ff00;
            font-size: 14px;
            font-weight: bold;
            text-align: center;
            flex-grow: 1;
        }

        .terminal-body {
            flex-grow: 1;
            padding: 20px;
            overflow-y: auto;
            background: #000;
        }

        .terminal-body::-webkit-scrollbar {
            width: 10px;
        }

        .terminal-body::-webkit-scrollbar-track {
            background: #0a0a0a;
        }

        .terminal-body::-webkit-scrollbar-thumb {
            background: #00ff00;
            border-radius: 5px;
        }

        .output-line {
            margin-bottom: 8px;
            line-height: 1.6;
            animation: fadeIn 0.3s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-5px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .command-line {
            color: #00ff00;
            font-weight: bold;
        }

        .result-line {
            color: #00d9ff;
            margin-right: 20px;
        }

        .error-line {
            color: #ff4444;
            margin-right: 20px;
        }

        .warning-line {
            color: #ffaa00;
            margin-right: 20px;
        }

        .success-line {
            color: #00ff88;
            margin-right: 20px;
        }

        .input-area {
            padding: 15px 20px;
            background: #0a0a0a;
            border-top: 2px solid #00ff00;
            display: flex;
            align-items: center;
        }

        .prompt {
            color: #00ff00;
            margin-left: 10px;
            font-weight: bold;
        }

        #commandInput {
            flex-grow: 1;
            background: transparent;
            border: none;
            color: #00ff00;
            font-family: 'Courier New', monospace;
            font-size: 16px;
            outline: none;
            direction: ltr;
            text-align: left;
        }

        .cursor {
            display: inline-block;
            width: 10px;
            height: 20px;
            background: #00ff00;
            animation: blink 1s infinite;
            margin-right: 5px;
        }

        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0; }
        }

        .ascii-art {
            color: #00ff00;
            text-align: center;
            font-size: 10px;
            line-height: 1.2;
            margin-bottom: 20px;
        }

        .loading {
            display: inline-block;
        }

        .loading::after {
            content: '...';
            animation: dots 1.5s infinite;
        }

        @keyframes dots {
            0%, 20% { content: '.'; }
            40% { content: '..'; }
            60%, 100% { content: '...'; }
        }

        .table {
            display: table;
            margin: 10px 0;
        }

        .table-row {
            display: table-row;
        }

        .table-cell {
            display: table-cell;
            padding: 5px 15px;
            border: 1px solid #00ff00;
        }

        .progress-bar {
            width: 300px;
            height: 20px;
            border: 1px solid #00ff00;
            margin: 10px 0;
            position: relative;
        }

        .progress-fill {
            height: 100%;
            background: #00ff00;
            transition: width 0.3s ease;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="window-controls">
                <button class="control-btn close"></button>
                <button class="control-btn minimize"></button>
                <button class="control-btn maximize"></button>
            </div>
            <div class="title">🖥️ Terminal Simulator v2.5.1</div>
            <div style="width: 70px;"></div>
        </div>

        <div class="terminal-body" id="terminalOutput">
            <div class="ascii-art">
 _____ _____ ____  __  __ ___ _   _    _    _     
|_   _| ____|  _ \|  \/  |_ _| \ | |  / \  | |    
  | | |  _| | |_) | |\/| || ||  \| | / _ \ | |    
  | | | |___|  _ <| |  | || || |\  |/ ___ \| |___ 
  |_| |_____|_| \_\_|  |_|___|_| \_/_/   \_\_____|
            </div>
            <div class="output-line success-line">✓ نظام التشغيل محمّل بنجاح</div>
            <div class="output-line success-line">✓ الاتصال بالشبكة مفعّل</div>
            <div class="output-line success-line">✓ جميع الأنظمة تعمل بشكل طبيعي</div>
            <div class="output-line" style="margin-top: 20px;">مرحباً بك في محاكي Terminal!</div>
            <div class="output-line">اكتب 'help' لعرض قائمة الأوامر المتاحة</div>
            <div class="output-line" style="margin-bottom: 20px;">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>
        </div>

        <div class="input-area">
            <span class="prompt">root@terminal:~$</span>
            <input type="text" id="commandInput" autofocus autocomplete="off">
            <span class="cursor"></span>
        </div>
    </div>

    <script>
        const output = document.getElementById('terminalOutput');
        const input = document.getElementById('commandInput');
        let commandHistory = [];
        let historyIndex = -1;

        const commands = {
            help: {
                desc: 'عرض قائمة الأوامر المتاحة',
                action: async () => {
                    await addOutput(`<div class="success-line">📋 الأوامر المتاحة:</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(70);
                    await addOutput(`<div class="result-line">🔹 ? - عرض هذه القائمة</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 clear - مسح الشاشة</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 sysinfo - معلومات النظام</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 scan [target] - فحص الهدف المحدد</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 network - عرض معلومات الشبكة</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 processes - عرض العمليات الجارية</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 encrypt [text] - تشفير النص</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 decrypt [text] - فك تشفير النص</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 ping [domain] - فحص الاتصال بالنطاق</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 date - عرض التاريخ والوقت</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 whoami - عرض معلومات المستخدم</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 matrix - تأثير Matrix</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">🔹 history - عرض سجل الأوامر</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    return '';
                }
            },
            clear: {
                desc: 'مسح الشاشة',
                action: () => {
                    output.innerHTML = '';
                    return '';
                }
            },
            sysinfo: {
                desc: 'عرض معلومات النظام',
                action: async () => {
                    await addOutput(`<div class="success-line">💻 معلومات النظام:</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(70);
                    await addOutput(`<div class="result-line">نظام التشغيل: TerminalOS v2.5.1</div>`);
                    await sleep(70);
                    await addOutput(`<div class="result-line">المعالج: Intel Core i9-13900K @ 5.8GHz</div>`);
                    await sleep(70);
                    await addOutput(`<div class="result-line">الذاكرة: 32GB DDR5</div>`);
                    await sleep(70);
                    await addOutput(`<div class="result-line">وقت التشغيل: ${Math.floor(Math.random() * 100)} يوم</div>`);
                    await sleep(70);
                    await addOutput(`<div class="result-line">الحالة: ✓ نشط</div>`);
                    await sleep(50);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    return '';
                }
            },
            scan: {
                desc: 'فحص هدف محدد',
                action: async (args) => {
                    const target = args[0] || 'localhost';
                    
                    await addOutput(`<div class="warning-line">🔍 جاري فحص: ${target}</div>`);
                    await sleep(800);
                    
                    await addOutput(`<div class="result-line">⏳ جاري البحث عن المنافذ المفتوحة...</div>`);
                    await sleep(1200);
                    
                    await addOutput(`<div class="success-line">✓ اكتمل الفحص بنجاح!</div>`);
                    await sleep(200);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(300);
                    
                    await addOutput(`<div class="result-line">المنفذ 80/HTTP - مفتوح ✓</div>`);
                    await sleep(400);
                    await addOutput(`<div class="result-line">المنفذ 443/HTTPS - مفتوح ✓</div>`);
                    await sleep(400);
                    await addOutput(`<div class="result-line">المنفذ 22/SSH - مفتوح ✓</div>`);
                    await sleep(400);
                    await addOutput(`<div class="result-line">المنفذ 3306/MySQL - مغلق ✗</div>`);
                    await sleep(300);
                    
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(200);
                    await addOutput(`<div class="success-line">النتيجة: 3/4 منافذ نشطة</div>`);
                    
                    return '';
                }
            },
            network: {
                desc: 'معلومات الشبكة',
                action: async () => {
                    await addOutput(`<div class="success-line">🌐 معلومات الشبكة:</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(150);
                    await addOutput(`<div class="result-line">عنوان IP: 192.168.1.${Math.floor(Math.random() * 255)}</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">Gateway: 192.168.1.1</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">DNS: 8.8.8.8, 8.8.4.4</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">سرعة التحميل: ${(Math.random() * 100).toFixed(2)} Mbps</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">سرعة الرفع: ${(Math.random() * 50).toFixed(2)} Mbps</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">الحالة: ✓ متصل</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    return '';
                }
            },
            processes: {
                desc: 'عرض العمليات',
                action: async () => {
                    const processes = [
                        ['PID', 'الاسم', 'استخدام CPU', 'الذاكرة'],
                        ['1234', 'systemd', '0.5%', '2.1 MB'],
                        ['5678', 'nginx', '2.3%', '45.2 MB'],
                        ['9012', 'mysql', '5.1%', '128.5 MB'],
                        ['3456', 'terminal', '1.2%', '12.8 MB']
                    ];
                    
                    await addOutput(`<div class="success-line">📊 العمليات النشطة:</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(200);
                    
                    for (let i = 0; i < processes.length; i++) {
                        let row = '<div class="table-row" style="display: flex; gap: 20px;">';
                        processes[i].forEach(cell => {
                            row += `<div style="color: ${i === 0 ? '#00ff00' : '#00d9ff'}; min-width: 120px;">${cell}</div>`;
                        });
                        row += '</div>';
                        await addOutput(row, false);
                        await sleep(200);
                    }
                    
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    return '';
                }
            },
            encrypt: {
                desc: 'تشفير نص',
                action: async (args) => {
                    if (!args[0]) {
                        await addOutput(`<div class="error-line">❌ خطأ: يرجى إدخال نص للتشفير</div>`);
                        return '';
                    }
                    const text = args.join(' ');
                    const encrypted = btoa(text);
                    
                    await addOutput(`<div class="success-line">🔐 تم التشفير بنجاح!</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(150);
                    await addOutput(`<div class="result-line">النص الأصلي: ${text}</div>`);
                    await sleep(200);
                    await addOutput(`<div class="warning-line">النص المشفر: ${encrypted}</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    return '';
                }
            },
            decrypt: {
                desc: 'فك تشفير نص',
                action: async (args) => {
                    if (!args[0]) {
                        await addOutput(`<div class="error-line">❌ خطأ: يرجى إدخال نص لفك التشفير</div>`);
                        return '';
                    }
                    try {
                        const decrypted = atob(args[0]);
                        await addOutput(`<div class="success-line">🔓 تم فك التشفير بنجاح!</div>`);
                        await sleep(100);
                        await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                        await sleep(150);
                        await addOutput(`<div class="warning-line">النص المشفر: ${args[0]}</div>`);
                        await sleep(200);
                        await addOutput(`<div class="result-line">النص المفكوك: ${decrypted}</div>`);
                        await sleep(100);
                        await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                        return '';
                    } catch (e) {
                        await addOutput(`<div class="error-line">❌ خطأ: نص مشفر غير صالح</div>`);
                        return '';
                    }
                }
            },
            ping: {
                desc: 'فحص الاتصال',
                action: async (args) => {
                    const domain = args[0] || 'google.com';
                    
                    await addOutput(`<div class="warning-line">📡 جاري اختبار الاتصال بـ ${domain}...</div>`);
                    await sleep(800);
                    
                    await addOutput(`<div class="success-line">✓ تم الاتصال بنجاح!</div>`);
                    await sleep(200);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(300);
                    
                    await addOutput(`<div class="result-line">64 bytes من ${domain}: time=${(Math.random() * 50).toFixed(2)}ms</div>`);
                    await sleep(200);
                    await addOutput(`<div class="result-line">64 bytes من ${domain}: time=${(Math.random() * 50).toFixed(2)}ms</div>`);
                    await sleep(200);
                    await addOutput(`<div class="result-line">64 bytes من ${domain}: time=${(Math.random() * 50).toFixed(2)}ms</div>`);
                    await sleep(200);
                    await addOutput(`<div class="result-line">64 bytes من ${domain}: time=${(Math.random() * 50).toFixed(2)}ms</div>`);
                    await sleep(200);
                    
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    
                    return '';
                }
            },
            date: {
                desc: 'عرض التاريخ والوقت',
                action: async () => {
                    const now = new Date();
                    await addOutput(`<div class="success-line">📅 ${now.toLocaleString('ar-SA')}</div>`);
                    return '';
                }
            },
            whoami: {
                desc: 'معلومات المستخدم',
                action: async () => {
                    await addOutput(`<div class="success-line">👤 معلومات المستخدم:</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(150);
                    await addOutput(`<div class="result-line">المستخدم: root</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">المجموعة: administrators</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">الصلاحيات: مدير نظام</div>`);
                    await sleep(150);
                    await addOutput(`<div class="result-line">المسار: /home/root</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    return '';
                }
            },
            matrix: {
                desc: 'تأثير Matrix',
                action: async () => {
                    for (let i = 0; i < 10; i++) {
                        let line = '';
                        for (let j = 0; j < 60; j++) {
                            line += Math.random() > 0.5 ? '1' : '0';
                        }
                        await addOutput(`<div class="success-line" style="font-size: 12px;">${line}</div>`, false);
                        await sleep(50);
                    }
                    return '';
                }
            },
            history: {
                desc: 'عرض السجل',
                action: async () => {
                    if (commandHistory.length === 0) {
                        await addOutput(`<div class="warning-line">⚠️ لا يوجد سجل أوامر</div>`);
                        return '';
                    }
                    await addOutput(`<div class="success-line">📜 سجل الأوامر:</div>`);
                    await sleep(100);
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    await sleep(150);
                    for (let i = 0; i < commandHistory.length; i++) {
                        await addOutput(`<div class="result-line">${i + 1}. ${commandHistory[i]}</div>`);
                        await sleep(80);
                    }
                    await addOutput(`<div class="result-line">━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</div>`, false);
                    return '';
                }
            }
        };

        function sleep(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }

        async function addOutput(html, typewriter = true) {
            const div = document.createElement('div');
            div.className = 'output-line';
            
            if (typewriter && html.length < 500) {
                // تأثير الكتابة للنصوص القصيرة
                output.appendChild(div);
                const tempDiv = document.createElement('div');
                tempDiv.innerHTML = html;
                const text = tempDiv.textContent || tempDiv.innerText;
                
                let currentHtml = '';
                const htmlParts = html.split('');
                
                for (let i = 0; i < htmlParts.length; i++) {
                    currentHtml += htmlParts[i];
                    div.innerHTML = currentHtml;
                    output.scrollTop = output.scrollHeight;
                    
                    // سرعة الكتابة - تخطي بعض التأخيرات للعلامات
                    if (htmlParts[i] === '>' || htmlParts[i] === '<') {
                        continue;
                    }
                    await sleep(Math.random() * 8 + 2); // سرعة أسرع
                }
            } else {
                // للنصوص الطويلة، اعرضها مباشرة
                div.innerHTML = html;
                output.appendChild(div);
            }
            
            output.scrollTop = output.scrollHeight;
        }

        async function executeCommand(cmdLine) {
            const parts = cmdLine.trim().split(' ');
            const cmd = parts[0].toLowerCase();
            const args = parts.slice(1);

            // عرض الأمر المكتوب بتأثير الكتابة
            const commandDiv = document.createElement('div');
            commandDiv.className = 'output-line';
            output.appendChild(commandDiv);
            
            const commandText = `root@terminal:~$ ${cmdLine}`;
            for (let i = 0; i < commandText.length; i++) {
                commandDiv.innerHTML = `<div class="command-line">${commandText.substring(0, i + 1)}</div>`;
                output.scrollTop = output.scrollHeight;
                await sleep(15); // أسرع
            }
            
            await sleep(100); // تقليل الانتظار

            if (cmdLine.trim() === '') return;

            commandHistory.push(cmdLine);
            historyIndex = commandHistory.length;

            if (commands[cmd]) {
                const result = await commands[cmd].action(args);
                if (result) await addOutput(result);
            } else {
                await addOutput(`<div class="error-line">❌ الأمر غير موجود: ${cmd}</div>`);
                await sleep(50);
                await addOutput(`<div class="warning-line">اكتب '?' لعرض الأوامر المتاحة</div>`);
            }
        }

        input.addEventListener('keydown', async (e) => {
            if (e.key === 'Enter') {
                const cmd = input.value;
                input.value = '';
                await executeCommand(cmd);
            } else if (e.key === 'ArrowUp') {
                e.preventDefault();
                if (historyIndex > 0) {
                    historyIndex--;
                    input.value = commandHistory[historyIndex];
                }
            } else if (e.key === 'ArrowDown') {
                e.preventDefault();
                if (historyIndex < commandHistory.length - 1) {
                    historyIndex++;
                    input.value = commandHistory[historyIndex];
                } else {
                    historyIndex = commandHistory.length;
                    input.value = '';
                }
            }
        });

        // تأكد من التركيز على حقل الإدخال
        document.addEventListener('click', () => input.focus());
    </script>
</body>
</html>
