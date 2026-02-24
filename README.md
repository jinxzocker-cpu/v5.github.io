# v5.github.io[velvet-boys.html](https://github.com/user-attachments/files/25531950/velvet-boys.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Velvet Boys Management System</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            overflow: hidden;
            background: #0a0a0a;
        }
        
        /* Login Screen */
        .login-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #1e3c72;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            transition: opacity 0.8s ease, transform 0.8s ease;
        }
        
        .login-screen.hidden {
            opacity: 0;
            pointer-events: none;
            transform: scale(1.1);
        }
        
        .password-container {
            position: relative;
            width: 320px;
            text-align: center;
        }
        
        .password-input {
            width: 100%;
            padding: 18px 24px;
            background: rgba(255,255,255,0.1);
            border: 2px solid rgba(255,255,255,0.2);
            border-radius: 30px;
            color: #fff;
            font-size: 16px;
            text-align: center;
            letter-spacing: 2px;
            outline: none;
            transition: all 0.3s ease;
        }
        
        .password-input:focus {
            border-color: rgba(255,255,255,0.5);
            background: rgba(255,255,255,0.15);
        }
        
        .password-input::placeholder {
            color: rgba(255,255,255,0.4);
        }
        
        .tries-warning {
            margin-top: 20px;
            color: #ffcc00;
            font-size: 14px;
            font-weight: 500;
            opacity: 0;
            transform: translateY(-10px);
            transition: all 0.3s ease;
        }
        
        .tries-warning.show {
            opacity: 1;
            transform: translateY(0);
        }
        
        .tries-warning.locked {
            color: #ff6b6b;
            font-size: 16px;
            font-weight: 600;
        }
        
        .error-shake {
            animation: shake 0.5s ease;
        }
        
        /* Desktop Environment */
        .desktop {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 50%, #1e3c72 100%);
            background-size: 400% 400%;
            animation: gradientShift 30s ease infinite;
            display: none;
            opacity: 0;
            transition: opacity 0.8s ease;
        }
        
        .desktop.active {
            display: block;
            opacity: 1;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .desktop-grid {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
            background-size: 50px 50px;
            pointer-events: none;
        }
        
        /* Desktop Icons */
        .desktop-icons {
            position: absolute;
            top: 40px;
            left: 40px;
            display: flex;
            flex-direction: column;
            gap: 30px;
        }
        
        .desktop-icon {
            width: 90px;
            text-align: center;
            cursor: pointer;
            transition: transform 0.2s ease;
            user-select: none;
        }
        
        .desktop-icon:hover {
            transform: scale(1.05);
        }
        
        .desktop-icon:active {
            transform: scale(0.95);
        }
        
        .icon-visual {
            width: 70px;
            height: 70px;
            margin: 0 auto 8px;
            background: linear-gradient(145deg, rgba(255,255,255,0.2), rgba(255,255,255,0.05));
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.2);
            box-shadow: 0 8px 32px rgba(0,0,0,0.2);
            transition: all 0.3s ease;
        }
        
        .desktop-icon:hover .icon-visual {
            background: linear-gradient(145deg, rgba(255,255,255,0.3), rgba(255,255,255,0.1));
            box-shadow: 0 12px 40px rgba(0,0,0,0.3);
            border-color: rgba(255,255,255,0.4);
        }
        
        .icon-label {
            color: #fff;
            font-size: 13px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.5);
            font-weight: 500;
            letter-spacing: 0.5px;
        }
        
        /* Taskbar */
        .taskbar {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 48px;
            background: rgba(20,20,30,0.9);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(255,255,255,0.1);
            display: flex;
            align-items: center;
            padding: 0 20px;
            gap: 20px;
            z-index: 100;
        }
        
        .start-btn {
            width: 40px;
            height: 40px;
            background: linear-gradient(145deg, #667eea 0%, #764ba2 100%);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s ease;
        }
        
        .start-btn:hover {
            transform: scale(1.1);
        }
        
        .taskbar-items {
            flex: 1;
            display: flex;
            gap: 10px;
        }
        
        .taskbar-item {
            padding: 8px 16px;
            background: rgba(255,255,255,0.05);
            border-radius: 6px;
            color: rgba(255,255,255,0.7);
            font-size: 13px;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 1px solid transparent;
        }
        
        .taskbar-item:hover {
            background: rgba(255,255,255,0.1);
            color: #fff;
        }
        
        .taskbar-item.active {
            background: rgba(255,255,255,0.15);
            border-color: rgba(255,255,255,0.3);
            color: #fff;
        }
        
        .clock {
            color: rgba(255,255,255,0.9);
            font-size: 13px;
            font-weight: 500;
            padding: 0 15px;
        }
        
        /* Window System */
        .window {
            position: absolute;
            background: rgba(30,30,40,0.95);
            border-radius: 12px;
            box-shadow: 0 25px 50px rgba(0,0,0,0.5);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255,255,255,0.1);
            min-width: 600px;
            min-height: 400px;
            display: none;
            flex-direction: column;
            overflow: hidden;
            animation: windowOpen 0.3s ease;
        }
        
        @keyframes windowOpen {
            from {
                opacity: 0;
                transform: scale(0.9) translateY(20px);
            }
            to {
                opacity: 1;
                transform: scale(1) translateY(0);
            }
        }
        
        .window.active {
            display: flex;
        }
        
        .window-header {
            height: 44px;
            background: rgba(0,0,0,0.3);
            display: flex;
            align-items: center;
            padding: 0 16px;
            gap: 12px;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            cursor: move;
        }
        
        .window-controls {
            display: flex;
            gap: 8px;
        }
        
        .window-btn {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            cursor: pointer;
            transition: transform 0.2s ease;
        }
        
        .window-btn:hover {
            transform: scale(1.2);
        }
        
        .window-btn.close { background: #ff5f57; }
        .window-btn.minimize { background: #febc2e; }
        .window-btn.maximize { background: #28c840; }
        
        .window-title {
            flex: 1;
            text-align: center;
            color: rgba(255,255,255,0.8);
            font-size: 14px;
            font-weight: 500;
            margin-left: -60px;
        }
        
        .window-content {
            flex: 1;
            padding: 24px;
            overflow-y: auto;
            color: #fff;
        }
        
        /* File Explorer Styles */
        .file-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
            gap: 20px;
        }
        
        .file-item {
            text-align: center;
            padding: 16px;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 1px solid transparent;
        }
        
        .file-item:hover {
            background: rgba(255,255,255,0.05);
            border-color: rgba(255,255,255,0.1);
            transform: translateY(-2px);
        }
        
        .file-icon {
            width: 60px;
            height: 70px;
            margin: 0 auto 10px;
            background: linear-gradient(145deg, #e74c3c 0%, #c0392b 100%);
            border-radius: 8px 8px 4px 4px;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: white;
            box-shadow: 0 4px 15px rgba(231, 76, 60, 0.3);
        }
        
        .file-icon::before {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 20px;
            height: 20px;
            background: linear-gradient(225deg, rgba(30,30,40,0.95) 50%, transparent 50%);
            border-radius: 0 8px 0 0;
        }
        
        .file-icon.pdf {
            background: linear-gradient(145deg, #e74c3c 0%, #c0392b 100%);
        }
        
        .file-icon.doc {
            background: linear-gradient(145deg, #3498db 0%, #2980b9 100%);
            box-shadow: 0 4px 15px rgba(52, 152, 219, 0.3);
        }
        
        .file-name {
            font-size: 12px;
            color: rgba(255,255,255,0.9);
            word-break: break-word;
            line-height: 1.4;
        }
        
        .file-meta {
            font-size: 11px;
            color: rgba(255,255,255,0.5);
            margin-top: 4px;
        }
        
        /* Sick Leave Document Styles */
        .doc-preview {
            background: #fff;
            color: #333;
            padding: 40px;
            border-radius: 8px;
            max-width: 600px;
            margin: 0 auto;
            font-family: 'Times New Roman', serif;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
        }
        
        .doc-header {
            text-align: center;
            border-bottom: 2px solid #1e3c72;
            padding-bottom: 20px;
            margin-bottom: 30px;
        }
        
        .doc-header h2 {
            color: #1e3c72;
            font-size: 24px;
            margin-bottom: 5px;
        }
        
        .doc-header p {
            color: #666;
            font-size: 14px;
        }
        
        .doc-field {
            margin-bottom: 20px;
            display: flex;
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
        }
        
        .doc-label {
            font-weight: bold;
            width: 150px;
            color: #1e3c72;
        }
        
        .doc-value {
            flex: 1;
            color: #333;
        }
        
        .doc-stamp {
            position: absolute;
            top: 40px;
            right: 40px;
            width: 100px;
            height: 100px;
            border: 3px solid #c0392b;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #c0392b;
            font-weight: bold;
            font-size: 14px;
            transform: rotate(-15deg);
            opacity: 0.7;
        }
        
        .back-btn {
            margin-bottom: 20px;
            padding: 10px 20px;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: #fff;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .back-btn:hover {
            background: rgba(255,255,255,0.2);
        }
        
        /* Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        
        ::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.1);
        }
        
        ::-webkit-scrollbar-thumb {
            background: rgba(255,255,255,0.2);
            border-radius: 4px;
        }
        
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(255,255,255,0.3);
        }
        
        /* Success Animation */
        .success-ring {
            position: absolute;
            width: 100px;
            height: 100px;
            border: 3px solid rgba(255,255,255,0);
            border-radius: 50%;
            animation: successPulse 0.6s ease forwards;
        }
        
        @keyframes successPulse {
            0% {
                transform: scale(0.8);
                border-color: rgba(255,255,255,0.8);
                opacity: 1;
            }
            100% {
                transform: scale(2);
                border-color: rgba(255,255,255,0);
                opacity: 0;
            }
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }
    </style>
</head>
<body>
    <!-- Login Screen -->
    <div class="login-screen" id="loginScreen">
        <div class="password-container">
            <input type="password" class="password-input" id="passwordInput" placeholder="Enter Password" maxlength="10">
            <div class="tries-warning" id="triesWarning"></div>
        </div>
    </div>
    
    <!-- Desktop Environment -->
    <div class="desktop" id="desktop">
        <div class="desktop-grid"></div>
        
        <div class="desktop-icons">
            <div class="desktop-icon" onclick="openWindow('members')">
                <div class="icon-visual">👥</div>
                <div class="icon-label">Members</div>
            </div>
            <div class="desktop-icon" onclick="openWindow('sickleave')">
                <div class="icon-visual">📋</div>
                <div class="icon-label">Sick Leaves</div>
            </div>
        </div>
        
        <!-- Members Window -->
        <div class="window" id="membersWindow" style="top: 80px; left: 100px;">
            <div class="window-header" onmousedown="startDrag(event, 'membersWindow')">
                <div class="window-controls">
                    <div class="window-btn close" onclick="closeWindow('members')"></div>
                    <div class="window-btn minimize" onclick="minimizeWindow('members')"></div>
                    <div class="window-btn maximize"></div>
                </div>
                <div class="window-title">Members</div>
            </div>
            <div class="window-content" id="membersContent">
                <div class="file-grid">
                    <div class="file-item" onclick="showPDF('Nick Ford')">
                        <div class="file-icon pdf">📄</div>
                        <div class="file-name">Nick Ford</div>
                        <div class="file-meta">PDF • 245 KB</div>
                    </div>
                    <div class="file-item" onclick="showPDF('Nathan Williams')">
                        <div class="file-icon pdf">📄</div>
                        <div class="file-name">Nathan Williams</div>
                        <div class="file-meta">PDF • 198 KB</div>
                    </div>
                    <div class="file-item" onclick="showPDF('Luke Raymond')">
                        <div class="file-icon pdf">📄</div>
                        <div class="file-name">Luke Raymond</div>
                        <div class="file-meta">PDF • 210 KB</div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Sick Leave Window -->
        <div class="window" id="sickleaveWindow" style="top: 100px; left: 150px;">
            <div class="window-header" onmousedown="startDrag(event, 'sickleaveWindow')">
                <div class="window-controls">
                    <div class="window-btn close" onclick="closeWindow('sickleave')"></div>
                    <div class="window-btn minimize" onclick="minimizeWindow('sickleave')"></div>
                    <div class="window-btn maximize"></div>
                </div>
                <div class="window-title">Sick Leaves</div>
            </div>
            <div class="window-content" id="sickleaveContent">
                <div class="file-grid" id="sickLeaveGrid">
                    <!-- Generated by JS -->
                </div>
            </div>
        </div>
        
        <!-- Taskbar -->
        <div class="taskbar">
            <div class="start-btn">VB</div>
            <div class="taskbar-items">
                <div class="taskbar-item" id="task-members" onclick="openWindow('members')">Members</div>
                <div class="taskbar-item" id="task-sickleave" onclick="openWindow('sickleave')">Sick Leaves</div>
            </div>
            <div class="clock" id="clock">12:00 PM</div>
        </div>
    </div>
    
    <script>
        // Password Protection
        const correctPassword = '02/06/2003';
        const passwordInput = document.getElementById('passwordInput');
        const triesWarning = document.getElementById('triesWarning');
        const loginScreen = document.getElementById('loginScreen');
        const desktop = document.getElementById('desktop');
        
        let attempts = 0;
        const maxAttempts = 3;
        let isLocked = false;
        
        passwordInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter' && !isLocked) {
                checkPassword();
            }
        });
        
        function checkPassword() {
            if (isLocked) return;
            
            attempts++;
            const remaining = maxAttempts - attempts;
            
            if (passwordInput.value === correctPassword) {
                // Success
                const ring = document.createElement('div');
                ring.className = 'success-ring';
                ring.style.left = '50%';
                ring.style.top = '50%';
                ring.style.marginLeft = '-50px';
                ring.style.marginTop = '-50px';
                loginScreen.appendChild(ring);
                
                setTimeout(() => {
                    loginScreen.classList.add('hidden');
                    desktop.classList.add('active');
                    updateClock();
                    setInterval(updateClock, 1000);
                    generateSickLeaves();
                }, 600);
            } else {
                // Failed attempt
                passwordInput.classList.add('error-shake');
                passwordInput.style.borderColor = '#ff6b6b';
                passwordInput.value = '';
                
                setTimeout(() => {
                    passwordInput.classList.remove('error-shake');
                    passwordInput.style.borderColor = '';
                }, 500);
                
                // Show warning only after first failed attempt
                if (attempts === 1) {
                    triesWarning.textContent = '2 more tries remaining';
                    triesWarning.classList.add('show');
                } else if (attempts === 2) {
                    triesWarning.textContent = '1 more try remaining';
                    triesWarning.classList.add('show');
                } else if (attempts >= 3) {
                    isLocked = true;
                    passwordInput.disabled = true;
                    passwordInput.placeholder = 'ACCESS DENIED';
                    triesWarning.textContent = 'SYSTEM LOCKED';
                    triesWarning.classList.remove('show');
                    triesWarning.classList.add('locked');
                    triesWarning.style.opacity = '1';
                    triesWarning.style.transform = 'translateY(0)';
                }
            }
        }
        
        // Window Management
        let activeWindow = null;
        let zIndex = 200;
        
        function openWindow(name) {
            const windowEl = document.getElementById(name + 'Window');
            const taskItem = document.getElementById('task-' + name);
            
            windowEl.style.display = 'flex';
            windowEl.style.zIndex = ++zIndex;
            windowEl.classList.add('active');
            
            document.querySelectorAll('.taskbar-item').forEach(item => item.classList.remove('active'));
            if (taskItem) taskItem.classList.add('active');
            
            activeWindow = name;
        }
        
        function closeWindow(name) {
            const windowEl = document.getElementById(name + 'Window');
            const taskItem = document.getElementById('task-' + name);
            
            windowEl.style.display = 'none';
            windowEl.classList.remove('active');
            if (taskItem) taskItem.classList.remove('active');
        }
        
        function minimizeWindow(name) {
            const windowEl = document.getElementById(name + 'Window');
            const taskItem = document.getElementById('task-' + name);
            
            windowEl.style.display = 'none';
            windowEl.classList.remove('active');
            if (taskItem) taskItem.classList.remove('active');
        }
        
        // Drag functionality
        let isDragging = false;
        let currentWindow = null;
        let offsetX = 0;
        let offsetY = 0;
        
        function startDrag(e, windowId) {
            isDragging = true;
            currentWindow = document.getElementById(windowId);
            currentWindow.style.zIndex = ++zIndex;
            
            const rect = currentWindow.getBoundingClientRect();
            offsetX = e.clientX - rect.left;
            offsetY = e.clientY - rect.top;
        }
        
        document.addEventListener('mousemove', function(e) {
            if (isDragging && currentWindow) {
                currentWindow.style.left = (e.clientX - offsetX) + 'px';
                currentWindow.style.top = (e.clientY - offsetY) + 'px';
            }
        });
        
        document.addEventListener('mouseup', function() {
            isDragging = false;
            currentWindow = null;
        });
        
        // Clock
        function updateClock() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('en-US', { 
                hour: 'numeric', 
                minute: '2-digit',
                hour12: true 
            });
            document.getElementById('clock').textContent = timeString;
        }
        
        // Generate Sick Leaves
        function generateSickLeaves() {
            const grid = document.getElementById('sickLeaveGrid');
            const nathanDates = ['2024-02-15', '2024-05-20', '2024-11-08'];
            const lukeDates = ['2024-03-10', '2024-07-15', '2025-01-22'];
            const nickDates = [
                '2024-02-05', '2024-02-28', '2024-03-15', '2024-04-02', 
                '2024-04-20', '2024-05-10', '2024-06-01', '2024-08-15',
                '2024-09-20', '2024-10-15', '2024-12-10', '2025-01-15',
                '2025-02-20', '2025-03-10', '2025-04-25', '2025-05-15'
            ];
            
            const nathanReasons = ['Common Cold - Vocal Rest Required', 'Minor Ankle Sprain - Physical Therapy', 'Seasonal Flu - Recovery Period'];
            const lukeReasons = ['Wrist Strain - Blade Handling Restriction', 'Lower Back Pain - Physical Therapy', 'Respiratory Infection - Rest Required'];
            const nickReasons = [
                'Psychological Care - Therapy Session', 'Anxiety Management - Professional Supervision', 'Cognitive Behavioral Therapy',
                'Stress Management - Clinical Support', 'Psychological Evaluation', 'Therapeutic Session - Mental Health',
                'Behavioral Pattern Analysis', 'Mindfulness Therapy Session', 'Psychiatric Consultation',
                'Emotional Regulation Therapy', 'Trauma Processing Session', 'Cognitive Restructuring',
                'Mental Health Maintenance', 'Therapeutic Intervention', 'Psychological Supervision',
                'Wellness Check - Professional Care'
            ];
            
            // Nathan's sick leaves
            nathanDates.forEach((date, i) => {
                grid.innerHTML += createSickLeaveItem('Nathan Williams', date, nathanReasons[i], 'NW-' + (i+1));
            });
            
            // Luke's sick leaves
            lukeDates.forEach((date, i) => {
                grid.innerHTML += createSickLeaveItem('Luke Raymond', date, lukeReasons[i], 'LR-' + (i+1));
            });
            
            // Nick's sick leaves
            nickDates.forEach((date, i) => {
                grid.innerHTML += createSickLeaveItem('Nick Ford', date, nickReasons[i], 'NF-' + (i+1));
            });
        }
        
        function createSickLeaveItem(name, date, reason, id) {
            const isNick = name === 'Nick Ford';
            const colorClass = isNick ? 'pdf' : 'doc';
            return `
                <div class="file-item" onclick="viewSickLeave('${name}', '${date}', '${reason}', '${id}')">
                    <div class="file-icon ${colorClass}">📋</div>
                    <div class="file-name">${name}</div>
                    <div class="file-meta">${date}</div>
                </div>
            `;
        }
        
        function viewSickLeave(name, date, reason, id) {
            const content = document.getElementById('sickleaveContent');
            const formattedDate = new Date(date).toLocaleDateString('en-US', { 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric' 
            });
            
            content.innerHTML = `
                <button class="back-btn" onclick="backToSickLeaves()">← Back to Files</button>
                <div style="position: relative;">
                    <div class="doc-preview">
                        <div class="doc-stamp">APPROVED</div>
                        <div class="doc-header">
                            <h2>MEDICAL CERTIFICATE</h2>
                            <p>Velvet Boys Management • Official Document</p>
                        </div>
                        
                        <div class="doc-field">
                            <div class="doc-label">Document ID:</div>
                            <div class="doc-value">SL-${id}-2024</div>
                        </div>
                        
                        <div class="doc-field">
                            <div class="doc-label">Member Name:</div>
                            <div class="doc-value">${name}</div>
                        </div>
                        
                        <div class="doc-field">
                            <div class="doc-label">Date of Issue:</div>
                            <div class="doc-value">${formattedDate}</div>
                        </div>
                        
                        <div class="doc-field">
                            <div class="doc-label">Duration:</div>
                            <div class="doc-value">5-7 Business Days</div>
                        </div>
                        
                        <div class="doc-field">
                            <div class="doc-label">Reason:</div>
                            <div class="doc-value">${reason}</div>
                        </div>
                        
                        <div class="doc-field">
                            <div class="doc-label">Physician:</div>
                            <div class="doc-value">Dr. Elena Vance, MD</div>
                        </div>
                        
                        <div class="doc-field">
                            <div class="doc-label">Status:</div>
                            <div class="doc-value" style="color: #27ae60; font-weight: bold;">APPROVED FOR LEAVE</div>
                        </div>
                        
                        <div style="margin-top: 40px; border-top: 1px solid #eee; padding-top: 20px; font-size: 12px; color: #999; text-align: center;">
                            This document is electronically generated and valid without signature.<br>
                            Velvet Boys Management System • Confidential
                        </div>
                    </div>
                </div>
            `;
        }
        
        function backToSickLeaves() {
            const content = document.getElementById('sickleaveContent');
            content.innerHTML = '<div class="file-grid" id="sickLeaveGrid"></div>';
            generateSickLeaves();
        }
        
        function showPDF(name) {
            const content = document.getElementById('membersContent');
            content.innerHTML = `
                <button class="back-btn" onclick="backToMembers()">← Back to Files</button>
                <div style="background: #fff; color: #333; padding: 40px; border-radius: 8px; max-width: 700px; margin: 0 auto; min-height: 600px; box-shadow: 0 10px 40px rgba(0,0,0,0.3); font-family: 'Georgia', serif;">
                    <div style="text-align: center; border-bottom: 3px solid #1a1a2e; padding-bottom: 30px; margin-bottom: 30px;">
                        <h1 style="color: #1a1a2e; font-size: 32px; margin-bottom: 10px; font-family: 'Cinzel', serif;">${name.toUpperCase()}</h1>
                        <p style="color: #666; font-size: 14px; letter-spacing: 2px;">VELVET BOYS • MEMBER PROFILE</p>
                    </div>
                    
                    <div style="display: flex; gap: 30px; margin-bottom: 30px;">
                        <div style="width: 150px; height: 200px; background: linear-gradient(145deg, #2a2a3e, #1a1a2e); border-radius: 8px; display: flex; align-items: center; justify-content: center; color: rgba(255,255,255,0.3); font-size: 60px;">
                            👤
                        </div>
                        <div style="flex: 1;">
                            <h3 style="color: #1a1a2e; margin-bottom: 15px; border-bottom: 1px solid #eee; padding-bottom: 10px;">Executive Summary</h3>
                            <p style="line-height: 1.6; color: #444; font-size: 14px; margin-bottom: 15px;">
                                ${getMemberSummary(name)}
                            </p>
                            <div style="background: #f8f9fa; padding: 15px; border-radius: 6px; border-left: 4px solid #1a1a2e;">
                                <strong>Symbolism:</strong> ${getMemberSymbol(name)}<br>
                                <strong>Status:</strong> <span style="color: #27ae60;">Active</span>
                            </div>
                        </div>
                    </div>
                    
                    <div style="margin-bottom: 30px;">
                        <h3 style="color: #1a1a2e; margin-bottom: 15px; border-bottom: 1px solid #eee; padding-bottom: 10px;">Professional Overview</h3>
                        <table style="width: 100%; border-collapse: collapse; font-size: 14px;">
                            <tr style="border-bottom: 1px solid #eee;">
                                <td style="padding: 12px; color: #666; width: 40%;">Primary Discipline</td>
                                <td style="padding: 12px; color: #333; font-weight: 500;">${getMemberDiscipline(name)}</td>
                            </tr>
                            <tr style="border-bottom: 1px solid #eee;">
                                <td style="padding: 12px; color: #666;">Join Date</td>
                                <td style="padding: 12px; color: #333;">${getMemberJoinDate(name)}</td>
                            </tr>
                            <tr style="border-bottom: 1px solid #eee;">
                                <td style="padding: 12px; color: #666;">Origin</td>
                                <td style="padding: 12px; color: #333;">${getMemberOrigin(name)}</td>
                            </tr>
                        </table>
                    </div>
                    
                    <div style="background: #f0f0f0; padding: 20px; border-radius: 8px; margin-top: 30px;">
                        <h4 style="color: #1a1a2e; margin-bottom: 10px;">Confidential Notes</h4>
                        <p style="font-size: 13px; color: #666; line-height: 1.5; font-style: italic;">
                            ${getMemberNotes(name)}
                        </p>
                    </div>
                    
                    <div style="text-align: center; margin-top: 40px; padding-top: 20px; border-top: 1px solid #eee; font-size: 11px; color: #999;">
                        Document ID: VB-2024-${name.split(' ')[0].toUpperCase()}-CONFIDENTIAL<br>
                        Generated: ${new Date().toLocaleDateString()}
                    </div>
                </div>
            `;
        }
        
        function backToMembers() {
            const content = document.getElementById('membersContent');
            content.innerHTML = `
                <div class="file-grid">
                    <div class="file-item" onclick="showPDF('Nick Ford')">
                        <div class="file-icon pdf">📄</div>
                        <div class="file-name">Nick Ford</div>
                        <div class="file-meta">PDF • 245 KB</div>
                    </div>
                    <div class="file-item" onclick="showPDF('Nathan Williams')">
                        <div class="file-icon pdf">📄</div>
                        <div class="file-name">Nathan Williams</div>
                        <div class="file-meta">PDF • 198 KB</div>
                    </div>
                    <div class="file-item" onclick="showPDF('Luke Raymond')">
                        <div class="file-icon pdf">📄</div>
                        <div class="file-name">Luke Raymond</div>
                        <div class="file-meta">PDF • 210 KB</div>
                    </div>
                </div>
            `;
        }
        
        function getMemberSummary(name) {
            const summaries = {
                'Nick Ford': 'Strategic mind behind Velvet Boys\' most intricate performances. Specialized in advanced card manipulation, psychological misdirection, and audience control. Currently under professional supervision for psychological difficulties, showing remarkable self-awareness and growth.',
                'Nathan Williams': 'The emotional centre of Velvet Boys. Through vocal control, dynamic range, and stage presence, transforms illusion into experience. Fan favourite since debut. Hopeless romantic subject to dating ban (exception made for Luke).',
                'Luke Raymond': 'Co-founder of Velvet Boys. Designs and executes high-risk precision performances specializing in blade choreography and mechanical illusion engineering. Receives higher shares than regular members. Fiancé of Carla.'
            };
            return summaries[name];
        }
        
        function getMemberSymbol(name) {
            const symbols = {
                'Nick Ford': 'King of Spades - Virgo',
                'Nathan Williams': 'Diamond Ace - Pisces',
                'Luke Raymond': 'King of Clubs - Capricorn'
            };
            return symbols[name];
        }
        
        function getMemberDiscipline(name) {
            const disciplines = {
                'Nick Ford': 'Advanced Card Manipulation & Psychological Illusion',
                'Nathan Williams': 'Vocal Performance & Audience Engagement',
                'Luke Raymond': 'Precision Blade Performance & Stage Engineering'
            };
            return disciplines[name];
        }
        
        function getMemberJoinDate(name) {
            const dates = {
                'Nick Ford': '17 January 2024',
                'Nathan Williams': '20 January 2024',
                'Luke Raymond': '02 November 2023 (Co-Founder)'
            };
            return dates[name];
        }
        
        function getMemberOrigin(name) {
            const origins = {
                'Nick Ford': 'Chicago, IL • 21.09.2002',
                'Nathan Williams': 'Miami, FL • 16.03.2002',
                'Luke Raymond': 'Atlanta, GA • 03.01.2002'
            };
            return origins[name];
        }
        
        function getMemberNotes(name) {
            const notes = {
                'Nick Ford': 'Member has shown significant improvement through structured routines. Requires controlled environments for optimal performance. His understanding of perception and control has deepened significantly through therapy.',
                'Nathan Williams': 'Carries 45% of group sales. Dating ban implemented to maintain focus, though member shows signs of romantic restlessness. Monitor for potential compliance issues.',
                'Luke Raymond': 'Highest risk member due to blade choreography. All performances require safety officer present. Relationship with Carla (founding connection) must not affect group dynamics.'
            };
            return notes[name];
        }
    </script>
</body>
</html>
