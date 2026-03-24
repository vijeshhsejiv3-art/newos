# newos
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NebulaOS | Gemini Edition</title>
    <style>
        /* ==============================
           CSS VARIABLES & RESET
           ============================== */
        :root {
            --bg-page: #050508;
            --glass: rgba(255, 255, 255, 0.03);
            --glass-heavy: rgba(10, 10, 15, 0.95);
            --glass-border: rgba(255, 255, 255, 0.08);
            --accent: #00a2ff;
            --gemini-gradient: linear-gradient(135deg, #4285f4, #9b72cb, #d96570);
            --browser-accent: #00f2ff;
            --notes-accent: #ffd700;
            --text-main: #ffffff;
            --text-muted: #888899;
            --blur: blur(30px);
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            --font-family: 'Inter', 'Segoe UI', system-ui, sans-serif;
            --bg-gradient: radial-gradient(at 0% 0%, rgba(66, 133, 244, 0.12) 0px, transparent 50%),
                           radial-gradient(at 100% 100%, rgba(155, 114, 203, 0.12) 0px, transparent 50%);
        }

        body.theme-crimson { --accent: #ff4d4d; --bg-gradient: radial-gradient(at 0% 0%, rgba(255, 77, 77, 0.15) 0px, transparent 50%), radial-gradient(at 100% 100%, rgba(100, 0, 0, 0.2) 0px, transparent 50%); }
        body.theme-emerald { --accent: #00ffaa; --bg-gradient: radial-gradient(at 0% 0%, rgba(0, 255, 170, 0.12) 0px, transparent 50%), radial-gradient(at 100% 100%, rgba(0, 50, 30, 0.2) 0px, transparent 50%); }
        body.theme-gold { --accent: #ffd700; --bg-gradient: radial-gradient(at 0% 0%, rgba(255, 215, 0, 0.1) 0px, transparent 50%), radial-gradient(at 100% 100%, rgba(50, 40, 0, 0.2) 0px, transparent 50%); }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: var(--font-family); outline: none; }
        body { height: 100vh; width: 100vw; overflow: hidden; background: var(--bg-page); color: var(--text-main); background-image: var(--bg-gradient); transition: background 1s ease; }
        .hidden { display: none !important; }

        /* DASHBOARD */
        #system-dashboard {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            display: grid;
            grid-template-columns: repeat(2, 260px);
            gap: 20px;
            z-index: 1;
        }

        .stat-card {
            background: var(--glass-heavy);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            padding: 18px;
            backdrop-filter: var(--blur);
            position: relative;
            transition: var(--transition);
        }
        
        .stat-card:hover { border-color: var(--accent); transform: scale(1.02); }

        .stat-title { font-size: 0.65rem; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); margin-bottom: 12px; display: flex; justify-content: space-between; align-items: center; }
        
        .core-input {
            width: 50px;
            background: rgba(0,0,0,0.3);
            border: 1px solid var(--glass-border);
            color: var(--accent);
            font-size: 0.8rem;
            text-align: right;
            border-radius: 4px;
            padding: 2px 5px;
        }

        .bar-container {
            width: 100%;
            height: 10px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            overflow: hidden;
            margin: 10px 0;
            border: 1px solid rgba(255, 255, 255, 0.02);
        }

        .bar-fill {
            height: 100%;
            width: 0%;
            border-radius: 10px;
            transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 0 10px currentColor;
        }

        .bar-value {
            font-size: 1.2rem;
            font-weight: 200;
            display: block;
        }

        /* LOGOS */
        .gemini-logo { width: 24px; height: 24px; background: var(--gemini-gradient); -webkit-mask: url("data:image/svg+xml,%3Csvg viewBox='0 0 24 24' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M12 0L14.59 9.41L24 12L14.59 14.59L12 24L9.41 14.59L0 12L9.41 9.41L12 0Z'/%3E%3C/svg%3E") no-repeat center; mask: url("data:image/svg+xml,%3Csvg viewBox='0 0 24 24' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M12 0L14.59 9.41L24 12L14.59 14.59L12 24L9.41 14.59L0 12L9.41 9.41L12 0Z'/%3E%3C/svg%3E") no-repeat center; mask-size: contain; display: inline-block; animation: gemini-pulse 4s infinite ease-in-out; }
        @keyframes gemini-pulse { 0%, 100% { transform: scale(1); filter: drop-shadow(0 0 5px rgba(155, 114, 203, 0.3)); } 50% { transform: scale(1.15); filter: drop-shadow(0 0 15px rgba(66, 133, 244, 0.6)); } }
        .browser-logo { width: 24px; height: 24px; border-radius: 50%; border: 1px solid rgba(0, 242, 255, 0.4); display: inline-flex; align-items: center; justify-content: center; position: relative; background: radial-gradient(circle, var(--browser-accent) 0%, transparent 70%); }
        .browser-logo::before { content: ''; position: absolute; width: 140%; height: 60%; border: 1.5px solid var(--browser-accent); border-radius: 50%; transform: rotateX(70deg); animation: orbit-spin 3s linear infinite; }
        .browser-logo::after { content: ''; width: 6px; height: 6px; background: #fff; border-radius: 50%; box-shadow: 0 0 10px #fff; }
        @keyframes orbit-spin { from { transform: rotateX(70deg) rotateZ(0deg); } to { transform: rotateX(70deg) rotateZ(360deg); } }
        .notes-logo { width: 24px; height: 24px; border: 1.5px solid var(--notes-accent); border-radius: 6px; position: relative; overflow: hidden; }
        .settings-logo { width: 24px; height: 24px; border: 2px solid var(--text-muted); border-radius: 50%; display: flex; align-items: center; justify-content: center; animation: spin 8s linear infinite; }
        .settings-logo::after { content: '⚙'; font-size: 18px; color: var(--text-muted); }
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        .logo-large { width: 52px; height: 52px; }

        /* WINDOWS */
        .window { position: absolute; min-width: 350px; background: var(--glass-heavy); backdrop-filter: var(--blur); border: 1px solid var(--glass-border); border-radius: 24px; box-shadow: 0 30px 80px rgba(0,0,0,0.7); display: flex; flex-direction: column; overflow: hidden; animation: winOpen 0.5s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes winOpen { from { opacity: 0; transform: scale(0.95) translateY(20px); } to { opacity: 1; transform: scale(1) translateY(0); } }
        .window-header { padding: 16px 22px; background: rgba(255,255,255,0.03); display: flex; justify-content: space-between; align-items: center; cursor: move; border-bottom: 1px solid var(--glass-border); }
        .window-content { flex: 1; overflow: hidden; background: rgba(0,0,0,0.2); }
        
        /* LOGIN SCREEN UPDATED */
        #login-screen { position: fixed; inset: 0; z-index: 10000; display: flex; align-items: center; justify-content: center; background: #050508; transition: 0.8s; }
        #login-screen.fade-out { opacity: 0; pointer-events: none; }
        
        .login-field {
            width: 100%;
            padding: 12px;
            margin-bottom: 10px;
            background: rgba(255,255,255,0.05);
            border: 1px solid var(--glass-border);
            border-radius: 8px;
            color: white;
            text-align: center;
        }

        /* DESKTOP */
        #desktop { height: 100vh; width: 100vw; position: relative; padding: 30px; }
        .desktop-icons { position: absolute; left: 30px; top: 30px; display: flex; flex-direction: column; gap: 25px; width: 100px; }
        .icon { width: 90px; padding: 12px; border-radius: 16px; text-align: center; cursor: pointer; transition: var(--transition); color: white; display: flex; flex-direction: column; align-items: center; }
        .icon:hover { background: var(--glass); transform: scale(1.05); }
        .icon-label { font-size: 0.8rem; margin-top: 10px; color: var(--text-muted); }
        #taskbar { position: fixed; bottom: 0; left: 0; right: 0; height: 60px; background: rgba(3, 3, 5, 0.9); backdrop-filter: blur(25px); display: flex; align-items: center; justify-content: space-between; padding: 0 30px; z-index: 9999; border-top: 1px solid var(--glass-border); }
        .chat-wrap { display: flex; flex-direction: column; height: 100%; }
        .chat-history { flex: 1; padding: 20px; overflow-y: auto; display: flex; flex-direction: column; gap: 15px; }
        .chat-msg { max-width: 80%; padding: 12px 16px; border-radius: 18px; font-size: 0.9rem; line-height: 1.4; }
        .chat-msg.ai { align-self: flex-start; background: rgba(255,255,255,0.05); border: 1px solid var(--glass-border); color: #fff; }
        .chat-msg.user { align-self: flex-end; background: var(--accent); color: #000; font-weight: 500; }
        .chat-input-bar { padding: 15px; border-top: 1px solid var(--glass-border); display: flex; gap: 10px; align-items: center; }
        input[type="text"], input[type="password"] { width: 100%; padding: 10px; border-radius: 8px; background: rgba(0,0,0,0.5); color: white; border: 1px solid var(--glass-border); }
        button.primary { padding: 10px 20px; border-radius: 8px; border: none; background: var(--gemini-gradient); color: white; cursor: pointer; font-weight: 600; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="login-card" style="text-align: center; width: 300px;">
            <div class="gemini-logo logo-large" style="margin-bottom: 20px;"></div>
            <h2 style="letter-spacing: 4px; margin-bottom: 5px;">NEBULA OS</h2>
            <p style="color: var(--text-muted); font-size: 0.7rem; margin-bottom: 25px;">GEMINI NEURAL CORE</p>
            
            <input type="text" id="login-id" class="login-field" placeholder="NEURAL ID">
            <input type="password" id="login-pass" class="login-field" placeholder="ACCESS CODE">
            
            <button class="primary" style="width: 100%;" onclick="System.login()">INITIALIZE</button>
            <p id="login-msg" style="color: #ff4d4d; font-size: 0.7rem; margin-top: 15px; height: 10px;"></p>
        </div>
    </div>

    <div id="desktop" class="hidden">
        <div id="system-dashboard">
            <div class="stat-card">
                <div class="stat-title">
                    <span>CPU CORE</span>
                    <input type="number" id="in-cpu" class="core-input" value="32" oninput="System.manualUpdate('cpu')">
                </div>
                <span class="bar-value" id="val-cpu" style="color: rgba(0, 162, 255, 1);">32%</span>
                <div class="bar-container">
                    <div id="bar-cpu" class="bar-fill" style="background: rgba(0, 162, 255, 1);"></div>
                </div>
            </div>
            <div class="stat-card">
                <div class="stat-title">
                    <span>GPU CORE</span>
                    <input type="number" id="in-gpu" class="core-input" value="18" oninput="System.manualUpdate('gpu')">
                </div>
                <span class="bar-value" id="val-gpu" style="color: rgba(155, 114, 203, 1);">18%</span>
                <div class="bar-container">
                    <div id="bar-gpu" class="bar-fill" style="background: rgba(155, 114, 203, 1);"></div>
                </div>
            </div>
            <div class="stat-card">
                <div class="stat-title">
                    <span>MEMORY</span>
                    <input type="number" id="in-mem" class="core-input" value="45" oninput="System.manualUpdate('mem')">
                </div>
                <span class="bar-value" id="val-mem" style="color: rgba(0, 255, 170, 1);">45%</span>
                <div class="bar-container">
                    <div id="bar-mem" class="bar-fill" style="background: rgba(0, 255, 170, 1);"></div>
                </div>
            </div>
            <div class="stat-card">
                <div class="stat-title">
                    <span>STORAGE</span>
                    <input type="number" id="in-storage" class="core-input" value="72" oninput="System.manualUpdate('storage')">
                </div>
                <span class="bar-value" id="val-storage" style="color: rgba(255, 215, 0, 1);">72%</span>
                <div class="bar-container">
                    <div id="bar-storage" class="bar-fill" style="background: rgba(255, 215, 0, 1);"></div>
                </div>
            </div>
        </div>

        <div class="desktop-icons">
            <div class="icon" onclick="WM.openApp('gemini')">
                <div class="gemini-logo logo-large"></div>
                <span class="icon-label">Neural Link</span>
            </div>
            <div class="icon" onclick="WM.openApp('browser')">
                <div class="browser-logo logo-large"></div>
                <span class="icon-label">Google</span>
            </div>
            <div class="icon" onclick="WM.openApp('notes')">
                <div class="notes-logo logo-large"></div>
                <span class="icon-label">Data Nodes</span>
            </div>
            <div class="icon" onclick="WM.openApp('settings')">
                <div class="settings-logo logo-large"></div>
                <span class="icon-label">Settings</span>
            </div>
        </div>

        <div id="window-layer"></div>

        <div id="taskbar">
            <div onclick="location.reload()" style="cursor:pointer; font-size:1.6rem;">💠</div>
            <div class="clock" id="clock">00:00:00</div>
        </div>
    </div>

    <script>
        let manualVals = { cpu: 32, gpu: 18, mem: 45, storage: 72 };

        const WM = {
            zStack: 100,
            apps: {
                settings: { title: "Settings", icon: '<div class="settings-logo"></div>', content: `<div style="padding:20px;"><h3>System Theme</h3><br><div class="theme-option" onclick="System.setTheme('crimson')" style="cursor:pointer; padding:10px; background:rgba(255,255,255,0.05); margin-bottom:10px; border-radius:8px;">Crimson Protocol</div><div class="theme-option" onclick="System.setTheme('emerald')" style="cursor:pointer; padding:10px; background:rgba(255,255,255,0.05); margin-bottom:10px; border-radius:8px;">Emerald Grid</div><div class="theme-option" onclick="System.setTheme('default')" style="cursor:pointer; padding:10px; background:rgba(255,255,255,0.05); border-radius:8px;">Nebula Default</div></div>` },
                browser: { title: "Quantum Orbit", icon: '<div class="browser-logo"></div>', content: `<div style="display:flex; flex-direction:column; height:100%; background:#fff;"><div style="background:#1a1a1c; padding:8px; display:flex; gap:8px; border-bottom:1px solid var(--glass-border);"><input type="text" id="browser-url-input" value="https://www.google.com/search?igu=1" style="flex:1; padding:6px 12px; font-size:12px; border-radius:6px; background:#000; color:#fff; border:1px solid #333;"><button class="primary" onclick="System.navigateBrowser()" style="padding:5px 15px; font-size:11px; white-space:nowrap;">GO</button></div><iframe id="main-browser-frame" src="https://www.google.com/search?igu=1" style="width:100%; flex:1; border:none; background:white;"></iframe></div>` },
                notes: { title: "Data Nodes", icon: '<div class="notes-logo"></div>', content: `<textarea style="width:100%; height:100%; background:transparent; color:var(--notes-accent); border:none; padding:20px; resize:none;" placeholder="Store encrypted node data..."></textarea>` },
                gemini: { title: "Neural Link", icon: '<div class="gemini-logo"></div>', content: `<div class="chat-wrap"><div class="chat-history" id="ai-chat"><div class="chat-msg ai">Core link stable.</div></div><div class="chat-input-bar"><input type="text" id="ai-input" placeholder="Transmit command..." onkeypress="if(event.key==='Enter') System.askAI()"><button class="primary" onclick="System.askAI()">SEND</button></div></div>` }
            },
            openApp(key) {
                const id = 'win-' + key;
                if(document.getElementById(id)) return this.focus(id);
                const app = this.apps[key];
                const win = document.createElement('div');
                win.className = 'window'; win.id = id; win.style.zIndex = ++this.zStack;
                win.style.top = (80 + (this.zStack % 5) * 30) + 'px'; 
                win.style.left = (150 + (this.zStack % 5) * 30) + 'px';
                win.style.width = key === 'browser' ? '850px' : (key === 'gemini' ? '450px' : '600px'); 
                win.style.height = '550px';
                win.innerHTML = `<div class="window-header" onmousedown="WM.drag(event, '${id}')"><div style="display:flex; align-items:center; gap:10px;">${app.icon} <span>${app.title}</span></div><span style="cursor:pointer; font-size:20px; opacity:0.5;" onclick="document.getElementById('${id}').remove()">×</span></div><div class="window-content">${app.content}</div>`;
                document.getElementById('window-layer').appendChild(win);
            },
            focus(id) { document.getElementById(id).style.zIndex = ++this.zStack; },
            drag(e, id) { const el = document.getElementById(id); this.focus(id); let p3=e.clientX, p4=e.clientY; document.onmousemove = (e) => { el.style.top = (el.offsetTop - (p4 - e.clientY)) + "px"; el.style.left = (el.offsetLeft - (p3 - e.clientX)) + "px"; p3=e.clientX; p4=e.clientY; }; document.onmouseup = () => { document.onmousemove = null; }; }
        };

        const System = {
            login() { 
                const id = document.getElementById('login-id').value;
                const pass = document.getElementById('login-pass').value;
                const msg = document.getElementById('login-msg');

                if (id === "Admin" && pass === "Gemini-2026") {
                    document.getElementById('login-screen').classList.add('fade-out');
                    setTimeout(() => {
                        document.getElementById('login-screen').classList.add('hidden');
                        document.getElementById('desktop').classList.remove('hidden');
                        this.refreshDashboard();
                    }, 800);
                } else {
                    msg.innerText = "INVALID NEURAL SIGNATURE";
                    setTimeout(() => { msg.innerText = ""; }, 2000);
                }
            },
            manualUpdate(key) {
                const val = document.getElementById('in-' + key).value;
                manualVals[key] = parseInt(val) || 0;
                this.refreshDashboard();
            },
            refreshDashboard() {
                Object.keys(manualVals).forEach(key => {
                    const bar = document.getElementById('bar-' + key);
                    const valText = document.getElementById('val-' + key);
                    if(bar) bar.style.width = manualVals[key] + '%';
                    if(valText) valText.innerText = manualVals[key] + '%';
                });
            },
            setTheme(name) { document.body.className = name === 'default' ? '' : 'theme-' + name; },
            navigateBrowser() { const input = document.getElementById('browser-url-input'); const frame = document.getElementById('main-browser-frame'); let url = input.value.trim(); if (!url.startsWith('http')) url = 'https://' + url; frame.src = url; },
            async askAI() { 
                const input = document.getElementById('ai-input'); 
                const chat = document.getElementById('ai-chat'); 
                if(!input.value.trim()) return;
                const userMsg = document.createElement('div'); userMsg.className = 'chat-msg user'; userMsg.textContent = input.value;
                chat.appendChild(userMsg); input.value = '';
                const aiMsg = document.createElement('div'); aiMsg.className = 'chat-msg ai'; aiMsg.textContent = "Processing...";
                chat.appendChild(aiMsg); chat.scrollTop = chat.scrollHeight;
                setTimeout(() => { aiMsg.textContent = "Neural connection verified. Interface active."; }, 1000);
            }
        };

        // Live Pulse Effect for Bar Graphs
        setInterval(() => {
            if(!document.getElementById('desktop').classList.contains('hidden')) {
                Object.keys(manualVals).forEach(key => {
                    const bar = document.getElementById('bar-' + key);
                    const jitter = (Math.random() * 2) - 1;
                    const displayVal = Math.max(0, Math.min(100, manualVals[key] + jitter));
                    if(bar) bar.style.width = displayVal + '%';
                });
            }
        }, 1500);

        setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
    </script>
</body>
</html>
