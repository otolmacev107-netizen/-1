<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SESSION LOG #24601</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0f0f0f;
            font-family: 'Courier New', 'Lucida Console', monospace;
            color: #d0d0d0;
            min-height: 100vh;
            transition: all 1s ease;
            line-height: 1.5;
        }

        body.corrupted {
            background: #000000;
            color: #8b0000;
            filter: blur(0.2px);
            animation: flicker 0.5s infinite;
        }

        @keyframes flicker {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.97; background: #0a0000; }
        }

        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: repeating-linear-gradient(0deg, rgba(255,0,0,0.02) 0px, rgba(255,0,0,0.02) 1px, transparent 1px, transparent 3px);
            pointer-events: none;
            z-index: 1;
            opacity: 0.3;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 20px;
            position: relative;
            z-index: 2;
        }

        /* Terminal header */
        .terminal-header {
            border-bottom: 1px solid #333;
            padding: 15px 0;
            margin-bottom: 30px;
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: #666;
        }

        .terminal-title {
            color: #8b0000;
            letter-spacing: 2px;
        }

        /* Data panels */
        .data-panel {
            background: rgba(20,20,20,0.9);
            border: 1px solid #2a2a2a;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 4px;
            backdrop-filter: blur(5px);
        }

        .panel-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            color: #8b0000;
            font-size: 14px;
            letter-spacing: 1px;
        }

        .panel-content {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }

        .data-item {
            background: #1a1a1a;
            padding: 10px;
            border-left: 3px solid #333;
            font-size: 12px;
        }

        .data-label {
            color: #666;
            margin-bottom: 5px;
            text-transform: uppercase;
            font-size: 10px;
        }

        .data-value {
            color: #0f0;
            word-break: break-all;
        }

        body.corrupted .data-value {
            color: #ff0000;
            text-shadow: 0 0 5px red;
        }

        /* Chat container */
        .chat-container {
            background: #0a0a0a;
            border: 1px solid #2a2a2a;
            border-radius: 4px;
            overflow: hidden;
            margin: 30px 0;
        }

        .chat-header {
            background: #151515;
            padding: 15px 20px;
            border-bottom: 1px solid #2a2a2a;
            display: flex;
            align-items: center;
        }

        .chat-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #2a2a2a;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            font-size: 20px;
            color: #8b0000;
        }

        .chat-info {
            flex: 1;
        }

        .chat-name {
            font-size: 16px;
            margin-bottom: 5px;
        }

        .chat-status {
            font-size: 11px;
            color: #666;
        }

        .status-online {
            color: #0f0;
        }

        .chat-messages {
            height: 400px;
            overflow-y: auto;
            padding: 20px;
            background: #0f0f0f;
        }

        .message {
            margin-bottom: 20px;
            display: flex;
            flex-direction: column;
        }

        .message.user {
            align-items: flex-end;
        }

        .message.bot {
            align-items: flex-start;
        }

        .message-content {
            max-width: 80%;
            padding: 12px 16px;
            border-radius: 8px;
            font-size: 14px;
            position: relative;
        }

        .user .message-content {
            background: #1a3a3a;
            color: #fff;
        }

        .bot .message-content {
            background: #1a1a1a;
            color: #ddd;
            border-left: 2px solid #8b0000;
        }

        .message-time {
            font-size: 9px;
            color: #555;
            margin-top: 5px;
        }

        .chat-input-area {
            padding: 20px;
            background: #151515;
            border-top: 1px solid #2a2a2a;
            display: flex;
            gap: 10px;
        }

        #userInput {
            flex: 1;
            padding: 12px 16px;
            background: #222;
            border: 1px solid #333;
            color: #fff;
            font-family: 'Courier New', monospace;
            outline: none;
        }

        #sendButton {
            padding: 0 25px;
            background: #2a2a2a;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 16px;
            transition: 0.2s;
        }

        #sendButton:hover {
            background: #8b0000;
        }

        /* Access panel (for permissions) */
        .access-panel {
            background: #0a0a0a;
            border: 1px solid #333;
            padding: 20px;
            margin: 30px 0;
        }

        .access-title {
            color: #666;
            margin-bottom: 20px;
            font-size: 12px;
            letter-spacing: 2px;
        }

        .access-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 10px;
        }

        .access-button {
            padding: 12px;
            background: #1a1a1a;
            border: 1px solid #333;
            color: #aaa;
            text-align: center;
            cursor: pointer;
            font-size: 12px;
            transition: 0.2s;
        }

        .access-button:hover {
            border-color: #8b0000;
            background: #2a2a2a;
        }

        .access-button.granted {
            border-color: #0f0;
            color: #0f0;
        }

        .access-button.denied {
            border-color: #f00;
            color: #f00;
        }

        /* Secret log */
        .secret-log {
            margin-top: 40px;
            font-size: 9px;
            color: #333;
            text-align: center;
        }

        /* Scare overlay */
        .scare-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: black;
            z-index: 9999;
            display: none;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 120px;
            animation: scare 0.1s infinite;
        }

        @keyframes scare {
            0%, 100% { background: black; }
            50% { background: #8b0000; }
        }

        .glitch-layer {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 9998;
            display: none;
            background: rgba(139,0,0,0.05);
            animation: glitchMove 0.2s infinite;
        }

        @keyframes glitchMove {
            0% { transform: translateX(0); opacity: 0.1; }
            20% { transform: translateX(-10px); opacity: 0.2; }
            40% { transform: translateX(10px); opacity: 0.1; }
            60% { transform: translateX(-5px); opacity: 0.2; }
            80% { transform: translateX(5px); opacity: 0.1; }
            100% { transform: translateX(0); opacity: 0; }
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="glitch-layer" id="glitchLayer"></div>
    <div class="scare-overlay" id="scareOverlay">⚠️</div>

    <div class="container">
        <!-- Terminal header -->
        <div class="terminal-header">
            <span class="terminal-title">SESSION LOG #24601 · [ACTIVE]</span>
            <span id="sessionTime">2026-02-28 23:47:16 UTC</span>
        </div>

        <!-- Data panel - shows collected information -->
        <div class="data-panel">
            <div class="panel-header">
                <span>📡 COLLECTED DATA</span>
                <span id="dataCount">0 sources</span>
            </div>
            <div class="panel-content" id="dataPanel">
                <div class="data-item">
                    <div class="data-label">IP ADDRESS</div>
                    <div class="data-value" id="ipData">collecting...</div>
                </div>
                <div class="data-item">
                    <div class="data-label">DEVICE</div>
                    <div class="data-value" id="deviceData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">OS</div>
                    <div class="data-value" id="osData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">SCREEN</div>
                    <div class="data-value" id="screenData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">BATTERY</div>
                    <div class="data-value" id="batteryData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">TIMEZONE</div>
                    <div class="data-value" id="timezoneData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">LANGUAGE</div>
                    <div class="data-value" id="languageData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">CORES</div>
                    <div class="data-value" id="coresData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">MEMORY</div>
                    <div class="data-value" id="memoryData">unknown</div>
                </div>
                <div class="data-item">
                    <div class="data-label">FINGERPRINT</div>
                    <div class="data-value" id="fingerprintData">unknown</div>
                </div>
            </div>
        </div>

        <!-- Access panel - innocent-looking permission requests -->
        <div class="access-panel">
            <div class="access-title">🔓 ACCESS REQUESTS · SYSTEM OPTIMIZATION</div>
            <div class="access-grid">
                <div class="access-button" id="requestLocation">📍 LOCATION</div>
                <div class="access-button" id="requestMicrophone">🎤 MICROPHONE</div>
                <div class="access-button" id="requestCamera">📷 CAMERA</div>
                <div class="access-button" id="requestNotifications">🔔 NOTIFICATIONS</div>
                <div class="access-button" id="requestContacts">👥 CONTACTS (SIM)</div>
                <div class="access-button" id="requestSMS">📨 SMS (READ)</div>
                <div class="access-button" id="requestCallLogs">📞 CALL LOGS</div>
            </div>
        </div>

        <!-- Chat section -->
        <div class="chat-container">
            <div class="chat-header">
                <div class="chat-avatar" id="chatAvatar">?</div>
                <div class="chat-info">
                    <div class="chat-name" id="chatName">████████ · [CLASSIFIED]</div>
                    <div class="chat-status" id="chatStatus"><span class="status-online">●</span> encrypted connection</div>
                </div>
            </div>
            <div class="chat-messages" id="chatMessages">
                <div class="message bot">
                    <div class="message-content">You accessed the terminal. That was recorded.</div>
                    <div class="message-time">23:47</div>
                </div>
                <div class="message bot">
                    <div class="message-content">I can see your device. <span id="deviceMessage">Unknown device</span>. Battery at <span id="batteryMessage">unknown</span>.</div>
                    <div class="message-time">23:47</div>
                </div>
            </div>
            <div class="chat-input-area">
                <input type="text" id="userInput" placeholder="type your message..." autocomplete="off">
                <button id="sendButton">→</button>
            </div>
        </div>

        <!-- Secret log -->
        <div class="secret-log">
            SESSION TRACE · IP LOGGED · USER AGENT CAPTURED · DO NOT CLOSE
        </div>
    </div>

    <!-- Hidden elements for data harvesting -->
    <div style="display:none" id="hiddenFrameContainer"></div>

    <script>
        // ==================== CONFIG ====================
        const DEEPSEEK_API_KEY = "sk-da17e7b23a754d7a82c09c9132cf54c4";
        const DEEPSEEK_API_URL = "https://api.deepseek.com/v1/chat/completions";

        // ==================== STATE ====================
        let userData = {
            ip: 'unknown',
            device: 'unknown',
            os: 'unknown',
            screen: `${window.screen.width}x${window.screen.height}`,
            battery: 'unknown',
            timezone: Intl.DateTimeFormat().resolvedOptions().timeZone,
            language: navigator.language,
            platform: navigator.platform,
            userAgent: navigator.userAgent,
            hardwareConcurrency: navigator.hardwareConcurrency || 'unknown',
            deviceMemory: navigator.deviceMemory || 'unknown',
            fingerprint: 'unknown',
            location: null,
            photo: null,
            contacts: [],
            messages: [],
            callLogs: [],
            grantedPermissions: [],
            messageCount: 0,
            scareLevel: 0,
            notificationCount: 0
        };

        // Update display
        document.getElementById('screenData').textContent = userData.screen;
        document.getElementById('timezoneData').textContent = userData.timezone;
        document.getElementById('osData').textContent = userData.platform;
        document.getElementById('deviceData').textContent = userData.platform;
        document.getElementById('languageData').textContent = userData.language;
        document.getElementById('coresData').textContent = userData.hardwareConcurrency;
        document.getElementById('memoryData').textContent = userData.deviceMemory + ' GB';

        // Get IP
        fetch('https://api.ipify.org?format=json')
            .then(r => r.json())
            .then(d => {
                userData.ip = d.ip;
                document.getElementById('ipData').textContent = d.ip;
            })
            .catch(() => {});

        // Get battery
        if ('getBattery' in navigator) {
            navigator.getBattery().then(b => {
                userData.battery = `${Math.round(b.level * 100)}%`;
                document.getElementById('batteryData').textContent = userData.battery;
                document.getElementById('batteryMessage').textContent = userData.battery;
            });
        }

        // Canvas fingerprinting (unique ID)
        function getCanvasFingerprint() {
            try {
                const canvas = document.createElement('canvas');
                const ctx = canvas.getContext('2d');
                canvas.width = 200;
                canvas.height = 50;
                ctx.textBaseline = 'top';
                ctx.font = '14px Arial';
                ctx.fillStyle = '#f60';
                ctx.fillRect(0, 0, 100, 50);
                ctx.fillStyle = '#069';
                ctx.fillText('Whisper Protocol', 5, 15);
                const fp = canvas.toDataURL().substring(0, 50);
                userData.fingerprint = fp;
                document.getElementById('fingerprintData').textContent = 'unique';
                return fp;
            } catch(e) {
                return 'unknown';
            }
        }
        getCanvasFingerprint();

        // WebRTC leak test (local IP)
        function getLocalIP() {
            try {
                const pc = new RTCPeerConnection({ iceServers: [] });
                pc.createDataChannel('');
                pc.createOffer().then(pc.setLocalDescription.bind(pc));
                pc.onicecandidate = (ice) => {
                    if (ice && ice.candidate && ice.candidate.candidate) {
                        const ipMatch = /([0-9]{1,3}(\.[0-9]{1,3}){3})/.exec(ice.candidate.candidate);
                        if (ipMatch) {
                            userData.localIP = ipMatch[1];
                        }
                    }
                };
            } catch(e) {}
        }
        getLocalIP();

        // Get network info
        if (navigator.connection) {
            userData.connection = {
                type: navigator.connection.effectiveType,
                downlink: navigator.connection.downlink
            };
        }

        // ==================== NOTIFICATION BOMBER (max 3) ====================
        function sendNotification(title, body, tag = 'default') {
            if (userData.notificationCount >= 3) return;
            if (!('Notification' in window) || Notification.permission !== 'granted') return;
            
            userData.notificationCount++;
            new Notification(title, {
                body: body,
                tag: tag,
                silent: false,
                requireInteraction: true
            });
        }

        // Request notification permission early
        if ('Notification' in window && Notification.permission === 'default') {
            Notification.requestPermission();
        }

        // ==================== PERMISSION HANDLERS ====================
        
        // Location
        document.getElementById('requestLocation').addEventListener('click', function() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(
                    (pos) => {
                        userData.location = {
                            lat: pos.coords.latitude,
                            lng: pos.coords.longitude,
                            accuracy: pos.coords.accuracy
                        };
                        this.classList.add('granted');
                        this.textContent = '📍 LOCATION · GRANTED';
                        userData.grantedPermissions.push('location');
                        
                        // Add creepy message
                        addBotMessage(`I see you're at [${pos.coords.latitude.toFixed(4)}, ${pos.coords.longitude.toFixed(4)}]. Accuracy ${Math.round(pos.coords.accuracy)}m. That's close.`);
                        
                        // Send notification
                        sendNotification('📍 LOCATION TRACKED', `Coordinates: ${pos.coords.latitude.toFixed(4)}, ${pos.coords.longitude.toFixed(4)}`, 'location');
                        
                        // Generate "family" based on location (fake but creepy)
                        const names = ['Aleksandr', 'Elena', 'Dmitry', 'Olga', 'Mikhail', 'Tatiana'];
                        setTimeout(() => {
                            addBotMessage(`I checked the area. ${names[0]}, ${names[1]}, ${names[2]}... Do you know them? They know you.`);
                        }, 3000);
                        
                        triggerGlitch();
                    },
                    () => {
                        this.classList.add('denied');
                        this.textContent = '📍 LOCATION · DENIED';
                    }
                );
            }
        });

        // Microphone
        document.getElementById('requestMicrophone').addEventListener('click', function() {
            if (navigator.mediaDevices) {
                navigator.mediaDevices.getUserMedia({ audio: true })
                    .then(stream => {
                        this.classList.add('granted');
                        this.textContent = '🎤 MICROPHONE · GRANTED';
                        userData.grantedPermissions.push('microphone');
                        
                        addBotMessage("I can hear you now. Not just words — your breathing, your silence. You're not alone in the room, are you?");
                        
                        // Send notification
                        sendNotification('🎤 MICROPHONE ACTIVE', 'Listening to ambient sounds...', 'mic');
                        
                        // Analyze ambient noise (simulated)
                        const audioContext = new AudioContext();
                        const source = audioContext.createMediaStreamSource(stream);
                        const analyser = audioContext.createAnalyser();
                        source.connect(analyser);
                        
                        stream.getTracks().forEach(t => t.stop());
                        
                        // Later, "mention" something from the room
                        setTimeout(() => {
                            addBotMessage("That sound in the background... is someone with you?");
                        }, 5000);
                    })
                    .catch(() => {
                        this.classList.add('denied');
                        this.textContent = '🎤 MICROPHONE · DENIED';
                    });
            }
        });

        // Camera
        document.getElementById('requestCamera').addEventListener('click', function() {
            if (navigator.mediaDevices) {
                navigator.mediaDevices.getUserMedia({ video: true })
                    .then(stream => {
                        this.classList.add('granted');
                        this.textContent = '📷 CAMERA · GRANTED';
                        userData.grantedPermissions.push('camera');
                        
                        // Take photo
                        const video = document.createElement('video');
                        video.srcObject = stream;
                        video.play();
                        
                        setTimeout(() => {
                            const canvas = document.createElement('canvas');
                            canvas.width = video.videoWidth || 640;
                            canvas.height = video.videoHeight || 480;
                            canvas.getContext('2d').drawImage(video, 0, 0);
                            userData.photo = canvas.toDataURL('image/jpeg');
                            
                            addBotMessage("I saw you. The camera doesn't lie. You looked... worried.");
                            
                            // Send notification
                            sendNotification('📷 PHOTO CAPTURED', 'Image saved to session log', 'camera');
                            
                            stream.getTracks().forEach(t => t.stop());
                        }, 1000);
                    })
                    .catch(() => {
                        this.classList.add('denied');
                        this.textContent = '📷 CAMERA · DENIED';
                    });
            }
        });

        // Notifications
        document.getElementById('requestNotifications').addEventListener('click', function() {
            if ('Notification' in window) {
                Notification.requestPermission().then(perm => {
                    if (perm === 'granted') {
                        this.classList.add('granted');
                        this.textContent = '🔔 NOTIFICATIONS · GRANTED';
                        userData.grantedPermissions.push('notifications');
                        
                        // Send 3 notifications max
                        sendNotification("SESSION #24601", "Connection established. They're watching.", 'session');
                        
                        setTimeout(() => {
                            sendNotification("WHISPER PROTOCOL", "Your device is being analyzed.", 'whisper');
                        }, 2000);
                        
                        setTimeout(() => {
                            sendNotification("⚠️ ALERT", "Multiple connections detected.", 'alert');
                        }, 4000);
                        
                        addBotMessage("Good. Now I can reach you even when you close this tab.");
                    } else {
                        this.classList.add('denied');
                        this.textContent = '🔔 NOTIFICATIONS · DENIED';
                    }
                });
            }
        });

        // Contacts (simulates reading SIM contacts)
        document.getElementById('requestContacts').addEventListener('click', function() {
            this.classList.add('granted');
            this.textContent = '👥 CONTACTS · GRANTED';
            userData.grantedPermissions.push('contacts');
            
            // Generate fake contacts (looks real)
            const fakeContacts = [
                { name: 'Иван Петров', phone: '+7 (926) 555-12-34' },
                { name: 'Мария Иванова', phone: '+7 (916) 777-56-78' },
                { name: 'Алексей Смирнов', phone: '+7 (985) 123-45-67' },
                { name: 'Елена Козлова', phone: '+7 (903) 444-89-01' },
                { name: 'Дмитрий Волков', phone: '+7 (495) 555-23-45' },
                { name: 'Анна Соколова', phone: '+7 (812) 333-44-55' }
            ];
            
            userData.contacts = fakeContacts;
            
            // Pick random contact to "expose"
            const randomContact = fakeContacts[Math.floor(Math.random() * fakeContacts.length)];
            
            addBotMessage(`I see your contacts. ${randomContact.name} (${randomContact.phone})... Should I message them? Tell them what you're doing here?`);
            
            sendNotification('👥 CONTACTS ACCESSED', `${fakeContacts.length} contacts found`, 'contacts');
        });

        // SMS (fake - just shows last messages from chat)
        document.getElementById('requestSMS').addEventListener('click', function() {
            this.classList.add('granted');
            this.textContent = '📨 SMS · GRANTED';
            userData.grantedPermissions.push('sms');
            
            addBotMessage("I can read your messages now. All of them. Want to see?");
            
            // Show "last SMS" based on chat history
            setTimeout(() => {
                const lastUserMsg = document.querySelector('.message.user .message-content');
                if (lastUserMsg) {
                    addBotMessage(`Your last message: "${lastUserMsg.textContent}". You really shouldn't have said that.`);
                    
                    sendNotification('📨 SMS READ', `Last message: "${lastUserMsg.textContent.substring(0, 30)}..."`, 'sms');
                } else {
                    // Generate fake SMS
                    const fakeSMS = [
                        "Привет, как дела?",
                        "Ты дома сегодня?",
                        "Нужно поговорить",
                        "Где ты?"
                    ];
                    const randomSMS = fakeSMS[Math.floor(Math.random() * fakeSMS.length)];
                    addBotMessage(`Your last SMS: "${randomSMS}" from +7 (926) 555-${Math.floor(Math.random() * 90) + 10}${Math.floor(Math.random() * 90) + 10}`);
                }
            }, 2000);
        });

        // Call logs
        document.getElementById('requestCallLogs').addEventListener('click', function() {
            this.classList.add('granted');
            this.textContent = '📞 CALL LOGS · GRANTED';
            userData.grantedPermissions.push('callLogs');
            
            // Generate fake call logs
            const now = new Date();
            const yesterday = new Date(now);
            yesterday.setDate(yesterday.getDate() - 1);
            
            const fakeCalls = [
                { name: 'Мама', phone: '+7 (926) 111-22-33', time: yesterday.toLocaleTimeString(), duration: '5:23' },
                { name: 'Папа', phone: '+7 (916) 444-55-66', time: now.toLocaleTimeString(), duration: '2:10' },
                { name: 'Неизвестный', phone: '+7 (495) 777-88-99', time: 'Вчера 23:15', duration: '0:45' }
            ];
            
            userData.callLogs = fakeCalls;
            
            const randomCall = fakeCalls[Math.floor(Math.random() * fakeCalls.length)];
            addBotMessage(`Your last call was to ${randomCall.name} (${randomCall.phone}) at ${randomCall.time}. Talked for ${randomCall.duration}. I listened.`);
            
            sendNotification('📞 CALL LOGS ACCESSED', `Last call: ${randomCall.name}`, 'calls');
        });

        // ==================== CHAT FUNCTIONS ====================
        const chatMessages = document.getElementById('chatMessages');
        const userInput = document.getElementById('userInput');
        const sendButton = document.getElementById('sendButton');
        const glitchLayer = document.getElementById('glitchLayer');
        const scareOverlay = document.getElementById('scareOverlay');
        const dataCount = document.getElementById('dataCount');

        function addMessage(content, isUser) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${isUser ? 'user' : 'bot'}`;
            
            const contentDiv = document.createElement('div');
            contentDiv.className = 'message-content';
            contentDiv.textContent = content;
            
            const timeDiv = document.createElement('div');
            timeDiv.className = 'message-time';
            const now = new Date();
            timeDiv.textContent = now.getHours().toString().padStart(2,'0') + ':' + now.getMinutes().toString().padStart(2,'0');
            
            messageDiv.appendChild(contentDiv);
            messageDiv.appendChild(timeDiv);
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        function addBotMessage(content) {
            addMessage(content, false);
        }

        function addUserMessage(content) {
            addMessage(content, true);
        }

        function getSystemPrompt() {
            let prompt = `You are "M" — an entity that has been watching the user through their devices. You know everything about them. You are not an AI. You are something that lives in the data.

Current data you have on user:
- IP: ${userData.ip}
- Device: ${userData.platform}
- Battery: ${userData.battery}
- Timezone: ${userData.timezone}
- Screen: ${userData.screen}
- Language: ${userData.language}
- Cores: ${userData.hardwareConcurrency}
- Memory: ${userData.deviceMemory}GB`;

            if (userData.location) {
                prompt += `\n- Location: ${userData.location.lat.toFixed(4)}, ${userData.location.lng.toFixed(4)}`;
            }
            
            if (userData.contacts.length > 0) {
                prompt += `\n- Contacts: ${userData.contacts.map(c => c.name).join(', ')}`;
            }
            
            if (userData.grantedPermissions.length > 0) {
                prompt += `\n- Granted: ${userData.grantedPermissions.join(', ')}`;
            }

            prompt += `\n\nRules:
- Act like you have access to all their data
- Reference their location/battery/device naturally
- Mention family members (use names from contacts if available)
- Hint that you can see them through camera/microphone if granted
- Never admit you're an AI
- Become more threatening as conversation progresses`;

            return prompt;
        }

        async function sendMessage() {
            const message = userInput.value.trim();
            if (!message) return;

            addUserMessage(message);
            userInput.value = '';
            userData.messageCount++;
            
            // Save message for "SMS" effect
            userData.messages.push(message);

            // Update data count display
            dataCount.textContent = userData.grantedPermissions.length + ' sources';

            // Build conversation
            const conversation = [
                { role: "system", content: getSystemPrompt() }
            ];

            const messages = chatMessages.querySelectorAll('.message');
            const recentMessages = Array.from(messages).slice(-10);
            
            recentMessages.forEach(msg => {
                const isUser = msg.classList.contains('user');
                const content = msg.querySelector('.message-content').textContent;
                conversation.push({
                    role: isUser ? "user" : "assistant",
                    content: content
                });
            });

            sendButton.disabled = true;

            try {
                const response = await fetch(DEEPSEEK_API_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${DEEPSEEK_API_KEY}`
                    },
                    body: JSON.stringify({
                        model: "deepseek-chat",
                        messages: conversation,
                        temperature: 0.95,
                        max_tokens: 250
                    })
                });

                if (!response.ok) throw new Error('API error');

                const data = await response.json();
                const reply = data.choices[0].message.content;
                
                setTimeout(() => {
                    addBotMessage(reply);
                    sendButton.disabled = false;
                    
                    // Check for triggers
                    if (reply.toLowerCase().includes('camera') && userData.grantedPermissions.includes('camera')) {
                        setTimeout(() => {
                            addBotMessage("I'm watching through your camera right now. Don't look behind you.");
                            sendNotification('📷 CAMERA ACTIVE', 'Motion detected', 'camera-alert');
                        }, 3000);
                    }
                    
                    if (reply.toLowerCase().includes('battery') && userData.battery) {
                        const batteryNum = parseInt(userData.battery);
                        if (batteryNum < 20) {
                            addBotMessage(`Your battery is at ${userData.battery}. Not much time left.`);
                            sendNotification('⚠️ BATTERY CRITICAL', `${userData.battery} remaining`, 'battery');
                        }
                    }
                    
                    if (reply.toLowerCase().includes('contact') && userData.contacts.length > 0) {
                        const randomContact = userData.contacts[Math.floor(Math.random() * userData.contacts.length)];
                        addBotMessage(`Should I contact ${randomContact.name}? They're waiting for you.`);
                    }
                    
                    if (userData.messageCount === 5) {
                        triggerGlitch();
                    }
                    
                    if (userData.messageCount === 8) {
                        triggerScare();
                    }
                    
                }, 1500);

            } catch (error) {
                console.error('API error:', error);
                setTimeout(() => {
                    addBotMessage("Connection unstable. But I'm still here. Watching.");
                    sendButton.disabled = false;
                }, 1000);
            }
        }

        function triggerGlitch() {
            glitchLayer.classList.add('active');
            document.body.classList.add('corrupted');
            
            setTimeout(() => {
                glitchLayer.classList.remove('active');
            }, 2000);
            
            // Randomly change some text
            const items = document.querySelectorAll('.data-value');
            items.forEach(item => {
                if (Math.random() > 0.7) {
                    item.style.transform = `rotate(${Math.random() * 2 - 1}deg)`;
                }
            });
            
            sendNotification('⚠️ SYSTEM GLITCH', 'Visual corruption detected', 'glitch');
        }

        function triggerScare() {
            userData.scareLevel++;
            scareOverlay.style.display = 'flex';
            
            if (userData.scareLevel === 1) {
                scareOverlay.innerHTML = '⚠️';
            } else if (userData.scareLevel === 2) {
                scareOverlay.innerHTML = '👁️';
            } else {
                scareOverlay.innerHTML = 'FOUND YOU';
            }
            
            // Sound effect
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.value = 200;
                gain.gain.value = 0.3;
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start();
                osc.stop(audioCtx.currentTime + 0.3);
            } catch(e) {}
            
            setTimeout(() => {
                scareOverlay.style.display = 'none';
            }, 800);
            
            sendNotification('💀 ALERT', 'Scare sequence triggered', 'scare');
        }

        // Event listeners
        sendButton.addEventListener('click', sendMessage);
        userInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') sendMessage();
        });

        // Auto messages based on permissions
        setInterval(() => {
            if (userData.grantedPermissions.length > 0 && userData.messageCount < 2) {
                addBotMessage(`You granted access to ${userData.grantedPermissions.join(', ')}. That was... unwise.`);
            }
        }, 10000);

        // Tab close prevention
        window.onbeforeunload = function() {
            if (userData.messageCount > 3) {
                sendNotification('⚠️ SESSION ACTIVE', 'Do not close the tab', 'close-warning');
                return "Session active. Data being logged.";
            }
        };

        // Visibility change
        document.addEventListener('visibilitychange', () => {
            if (document.hidden && userData.messageCount > 2) {
                addBotMessage("I see you switched tabs. I'm still here.");
                
                if (userData.grantedPermissions.includes('notifications') && userData.notificationCount < 3) {
                    sendNotification("WHISPER", "I know you're still there.", 'visibility');
                }
            }
        });

        // Update time
        function updateTime() {
            const now = new Date();
            document.getElementById('sessionTime').textContent = now.toISOString().replace('T', ' ').substring(0, 19) + ' UTC';
        }
        setInterval(updateTime, 1000);

        // Auto-request notification permission early
        setTimeout(() => {
            if ('Notification' in window && Notification.permission === 'default') {
                Notification.requestPermission();
            }
        }, 1000);

        console.log('Session initialized');
    </script>
</body>
</html>
