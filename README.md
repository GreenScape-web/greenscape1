<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI8 Super Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, orderBy, serverTimestamp, doc, deleteDoc, getDocs } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // Set log level to debug for better console output
        setLogLevel('debug');

        // Global variables provided by the environment
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        let app, db, auth, userId;
        let isAuthReady = false;
        let isSending = false;
        let userScrolledUp = false; // New variable to track user scroll position
        const messagesDiv = document.getElementById('messages');
        const userInput = document.getElementById('userInput');
        const chatBox = document.getElementById('chatBox');
        const settingsPanel = document.getElementById('settingsPanel');
        const newChatBtn = document.getElementById('newChatBtn');
        const downloadChatBtn = document.getElementById('downloadChatBtn');
        const userIdSpan = document.getElementById('userId');
        const loadingIndicator = document.getElementById('loadingIndicator');
        const scrollToBottomBtn = document.getElementById('scrollToBottomBtn');

        // Initialize Firebase on DOMContentLoaded
        document.addEventListener('DOMContentLoaded', async () => {
            try {
                if (Object.keys(firebaseConfig).length) {
                    app = initializeApp(firebaseConfig);
                    db = getFirestore(app);
                    auth = getAuth(app);
                    
                    // Add scroll event listener to the chat box
                    chatBox.addEventListener('scroll', handleScroll);

                    // Sign in with custom token or anonymously
                    await onAuthStateChanged(auth, async (user) => {
                        if (user) {
                            userId = user.uid;
                            isAuthReady = true;
                            if (userIdSpan) userIdSpan.textContent = `ID: ${userId}`;
                            console.log("Firebase Auth State Changed. User ID:", userId);
                            listenForMessages();
                            userInput.removeAttribute('readonly');
                        } else {
                            if (initialAuthToken) {
                                await signInWithCustomToken(auth, initialAuthToken);
                            } else {
                                await signInAnonymously(auth);
                            }
                        }
                    });
                } else {
                    console.error("Firebase config is missing.");
                    userInput.setAttribute('readonly', true);
                    // Use a custom message box instead of alert()
                    showMessageBox('Firebase is not configured. Please check the provided environment variables.');
                }
            } catch (e) {
                console.error("Firebase initialization failed:", e);
                userInput.setAttribute('readonly', true);
                    // Use a custom message box instead of alert()
                    showMessageBox('An error occurred during Firebase initialization. Please check the console for details.');
                }
            });
            
            // Function to handle chat box scrolling
            function handleScroll() {
                const atBottom = chatBox.scrollTop + chatBox.clientHeight >= chatBox.scrollHeight - 10;
                userScrolledUp = !atBottom;
                if (userScrolledUp) {
                    scrollToBottomBtn.classList.remove('hidden');
                } else {
                    scrollToBottomBtn.classList.add('hidden');
                }
            }

            // Function to scroll to the bottom of the chat
            function scrollToBottom() {
                chatBox.scrollTop = chatBox.scrollHeight;
            }

            // Function to listen for real-time messages from Firestore
            function listenForMessages() {
                if (!db || !isAuthReady) return;
                
                const publicChatCollectionRef = collection(db, 'artifacts', appId, 'public', 'data', 'chat_messages');
                const q = query(publicChatCollectionRef, orderBy('timestamp'));
                
                // This approach ensures correct ordering by re-rendering the entire chat history on every update
                onSnapshot(q, (snapshot) => {
                    // Clear existing messages to prevent duplicates and ensure correct order
                    messagesDiv.innerHTML = '';
                    
                    snapshot.forEach(doc => {
                        const msg = doc.data();
                        renderSingleMessage(msg);
                    });
                    // Only scroll to the bottom if the user hasn't scrolled up
                    if (!userScrolledUp) {
                         scrollToBottom();
                    }
                }, (error) => {
                    console.error("Failed to fetch messages:", error);
                });
            }
            
            // This function now adds a single message to the DOM
            function renderSingleMessage(msg) {
                const messageDiv = document.createElement('div');
                messageDiv.className = `p-3 rounded-xl max-w-[75%] break-words relative opacity-0 transform translate-y-5 transition-all duration-300 ${msg.role === 'user' ? 'bg-cyan-700 text-white self-end' : 'bg-gray-700 text-gray-50 self-start'}`;
                messageDiv.textContent = msg.text;
                messagesDiv.appendChild(messageDiv);
                // Animate messages
                setTimeout(() => messageDiv.style.opacity = 1, 100);
            }
            
            async function sendPrompt(prompt, systemInstruction) {
                isSending = true;
                loadingIndicator.classList.remove('hidden');
                
                try {
                    const apiKey = "";
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
                    const payload = {
                        contents: [{ parts: [{ text: prompt }] }],
                        tools: [{ "google_search": {} }],
                        systemInstruction: { parts: [{ text: systemInstruction }] },
                    };
                    
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    
                    if (!response.ok) throw new Error(`API error: ${response.statusText}`);
                    
                    const result = await response.json();
                    const candidate = result.candidates?.[0];
                    const aiResponseText = candidate?.content?.parts?.[0]?.text || "Sorry, I couldn't generate a response.";
                    
                    userScrolledUp = false; // Force scroll to bottom for AI's response
                    await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chat_messages'), {
                        text: aiResponseText,
                        role: 'ai',
                        timestamp: serverTimestamp()
                    });
                } catch (err) {
                    console.error('An error occurred:', err);
                    if (db && isAuthReady) {
                        await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chat_messages'), {
                            text: 'حدث خطأ، حاول لاحقًا.',
                            role: 'ai',
                            timestamp: serverTimestamp()
                        });
                    }
                } finally {
                    isSending = false;
                    loadingIndicator.classList.add('hidden');
                }
            }
            
            // Main function to send user message to AI
            async function sendMessage() {
                if (isSending || userInput.value.trim() === '') return;
                const userMessage = userInput.value.trim();
                userInput.value = '';
                
                userScrolledUp = false; // Force scroll to bottom for user's own message
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chat_messages'), {
                    text: userMessage,
                    role: 'user',
                    timestamp: serverTimestamp()
                });
                
                sendPrompt(userMessage, "You are a helpful assistant named AI8.");
            }
            
            // New function to summarize a text
            async function summarizeText() {
                if (isSending || userInput.value.trim() === '') return;
                const userMessage = userInput.value.trim();
                userInput.value = '';
                
                userScrolledUp = false; // Force scroll to bottom
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chat_messages'), {
                    text: `✨ تلخيص النص: ${userMessage.substring(0, 50)}...`,
                    role: 'user',
                    timestamp: serverTimestamp()
                });
                sendPrompt(userMessage, "Please summarize the following text concisely.");
            }
            
            // New function to handle Text-to-Speech
            async function speakLastMessage() {
                if (isSending) return;
                
                // Get the last message from the DOM
                const lastMessageElement = messagesDiv.lastElementChild;
                if (!lastMessageElement || lastMessageElement.className.includes('user')) {
                    return;
                }
                const textToSpeak = lastMessageElement.textContent;
                
                try {
                    const apiKey = "";
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`;
                    const payload = {
                        contents: [{
                            parts: [{ text: textToSpeak }]
                        }],
                        generationConfig: {
                            responseModalities: ["AUDIO"],
                            speechConfig: {
                                voiceConfig: {
                                    prebuiltVoiceConfig: { voiceName: "Kore" }
                                }
                            }
                        },
                        model: "gemini-2.5-flash-preview-tts"
                    };
                    
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    
                    const result = await response.json();
                    const part = result?.candidates?.[0]?.content?.parts?.[0];
                    const audioData = part?.inlineData?.data;
                    const mimeType = part?.inlineData?.mimeType;
                    
                    if (audioData && mimeType && mimeType.startsWith("audio/")) {
                        const sampleRate = parseInt(mimeType.match(/rate=(\d+)/)[1], 10);
                        const pcmData = base64ToArrayBuffer(audioData);
                        const pcm16 = new Int16Array(pcmData);
                        const wavBlob = pcmToWav(pcm16, sampleRate);
                        const audioUrl = URL.createObjectURL(wavBlob);
                        
                        const audio = new Audio(audioUrl);
                        audio.play();
                    } else {
                        console.error("Failed to get audio data.");
                    }
                } catch (err) {
                    console.error('TTS error:', err);
                }
            }
            
            // Helper function to convert base64 to ArrayBuffer
            function base64ToArrayBuffer(base64) {
                const binaryString = window.atob(base64);
                const len = binaryString.length;
                const bytes = new Uint8Array(len);
                for (let i = 0; i < len; i++) {
                    bytes[i] = binaryString.charCodeAt(i);
                }
                return bytes.buffer;
            }
            
            // Helper function to convert PCM to WAV format
            function pcmToWav(pcmData, sampleRate) {
                const numChannels = 1;
                const bitsPerSample = 16;
                const byteRate = (bitsPerSample / 8) * numChannels * sampleRate;
                const blockAlign = (bitsPerSample / 8) * numChannels;
                
                const buffer = new ArrayBuffer(44 + pcmData.byteLength);
                const view = new DataView(buffer);
                let offset = 0;
                
                // RIFF header
                writeString(view, offset, 'RIFF'); offset += 4;
                view.setUint32(offset, 36 + pcmData.byteLength, true); offset += 4;
                writeString(view, offset, 'WAVE'); offset += 4;
                
                // fmt sub-chunk
                writeString(view, offset, 'fmt '); offset += 4;
                view.setUint32(offset, 16, true); offset += 4;
                view.setUint16(offset, 1, true); offset += 2; // Audio format (1 for PCM)
                view.setUint16(offset, numChannels, true); offset += 2;
                view.setUint32(offset, sampleRate, true); offset += 4;
                view.setUint32(offset, byteRate, true); offset += 4;
                view.setUint16(offset, blockAlign, true); offset += 2;
                view.setUint16(offset, bitsPerSample, true); offset += 2;
                
                // data sub-chunk
                writeString(view, offset, 'data'); offset += 4;
                view.setUint32(offset, pcmData.byteLength, true); offset += 4;
                
                // Write PCM data
                const pcmBytes = new Uint8Array(pcmData.buffer);
                for (let i = 0; i < pcmBytes.length; i++, offset++) {
                    view.setUint8(offset, pcmBytes[i]);
                }
                
                return new Blob([view], { type: 'audio/wav' });
            }
            
            function writeString(view, offset, string) {
                for (let i = 0; i < string.length; i++) {
                    view.setUint8(offset + i, string.charCodeAt(i));
                }
            }
            
            async function newChat() {
                if (!db || !isAuthReady) return;
                const publicChatCollectionRef = collection(db, 'artifacts', appId, 'public', 'data', 'chat_messages');
                const snapshot = await getDocs(publicChatCollectionRef);
                const deletePromises = [];
                snapshot.forEach(doc => {
                    deletePromises.push(deleteDoc(doc.ref));
                });
                await Promise.all(deletePromises);
                console.log('Chat history cleared.');
            }
            
            function toggleSettings() {
                settingsPanel.classList.toggle('hidden');
            }
            
            // Add a simple message box to handle errors without using alert()
            function showMessageBox(message) {
                const messageBox = document.createElement('div');
                messageBox.className = 'fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 bg-gray-900 text-white p-6 rounded-lg shadow-2xl z-[100] border border-cyan-400';
                messageBox.innerHTML = `
                <p>${message}</p>
                <button class="mt-4 bg-cyan-600 hover:bg-cyan-700 text-white font-bold py-2 px-4 rounded-lg transition-colors" onclick="this.parentNode.remove()">حسنا</button>
                `;
                document.body.appendChild(messageBox);
            }
            
            // New function to download chat history
            async function downloadChatHistory() {
                if (!db || !isAuthReady) return;
                const publicChatCollectionRef = collection(db, 'artifacts', appId, 'public', 'data', 'chat_messages');
                const q = query(publicChatCollectionRef, orderBy('timestamp'));
                
                try {
                    const querySnapshot = await getDocs(q);
                    let chatContent = "سجل محادثة AI8\n\n";
                    
                    querySnapshot.forEach(doc => {
                        const msg = doc.data();
                        const sender = msg.role === 'user' ? 'أنت' : 'AI8';
                        chatContent += `${sender}: ${msg.text}\n`;
                    });
                    
                    const blob = new Blob([chatContent], { type: 'text/plain;charset=utf-8' });
                    const url = URL.createObjectURL(blob);
                    const link = document.createElement('a');
                    link.href = url;
                    link.download = 'AI8_chat_history.txt';
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                    URL.revokeObjectURL(url);
                    
                } catch (error) {
                    console.error("Failed to download chat history:", error);
                    showMessageBox('فشل تنزيل المحادثة. حاول مرة أخرى.');
                }
            }

            // Add event listeners for new UI buttons
            document.getElementById('sendBtn').addEventListener('click', sendMessage);
            document.getElementById('summarizeBtn').addEventListener('click', summarizeText);
            document.getElementById('speakBtn').addEventListener('click', speakLastMessage);
            document.getElementById('downloadChatBtn').addEventListener('click', downloadChatHistory);
            document.getElementById('scrollToBottomBtn').addEventListener('click', scrollToBottom);
            
            userInput.addEventListener('keydown', (e) => {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    sendMessage();
                }
            });
            document.getElementById('settingsBtn').addEventListener('click', toggleSettings);
            document.getElementById('closeSettingsBtn').addEventListener('click', toggleSettings);
            newChatBtn.addEventListener('click', newChat);
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap');
        body {
            font-family: 'Cairo', sans-serif;
            margin: 0;
            background: #0c0c0c;
            color: #fff;
            display: flex;
            flex-direction: column;
            height: 100vh;
        }
        .chat-box::-webkit-scrollbar {
            width: 8px;
        }
        .chat-box::-webkit-scrollbar-thumb {
            background-color: #4a4a4a;
            border-radius: 4px;
        }
        .chat-box::-webkit-scrollbar-track {
            background-color: #1a1a1a;
        }
        .message:first-of-type {
            margin-top: auto; /* Ensures messages are at the bottom */
        }
    </style>
</head>
<body class="bg-gray-950 text-white">

    <!-- Header -->
    <header class="fixed top-0 left-0 w-full h-16 p-4 bg-gray-900 text-cyan-400 flex justify-between items-center z-50 shadow-lg shadow-cyan-900/50">
        <h1 class="text-2xl font-bold">AI8</h1>
        <div class="flex items-center gap-2 text-sm text-gray-400">
            <span id="userId"></span>
            <button id="settingsBtn" class="text-3xl transition-transform hover:rotate-90 duration-300">⚙️</button>
        </div>
    </header>

    <!-- Settings Panel (RTL) -->
    <div id="settingsPanel" class="fixed top-0 right-0 w-80 h-full bg-gray-900 text-white p-6 transform translate-x-full transition-transform duration-300 hidden z-40 overflow-y-auto shadow-xl shadow-cyan-900/50">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-bold text-cyan-400">الإعدادات</h2>
            <button id="closeSettingsBtn" class="text-gray-400 text-2xl hover:text-white transition-colors">&times;</button>
        </div>
        <div class="space-y-4">
            <button id="newChatBtn" class="w-full bg-cyan-700 hover:bg-cyan-800 text-white font-bold py-3 px-6 rounded-lg transition-colors">محادثة جديدة</button>
            <button id="downloadChatBtn" class="w-full bg-green-700 hover:bg-green-800 text-white font-bold py-3 px-6 rounded-lg transition-colors">تنزيل المحادثة</button>
            <!-- Other settings can be added here, but the core functionality is handled by Firestore -->
        </div>
    </div>

    <!-- Main Chat Container -->
    <div class="flex-1 flex justify-center items-end mt-16 mb-2">
        <div id="chatBox" class="w-full max-w-2xl h-full mx-2 p-4 flex flex-col bg-gray-800 rounded-3xl shadow-2xl shadow-cyan-900/50 overflow-y-auto">
            <div id="messages" class="flex flex-col gap-2 p-2">
                <!-- Messages will be rendered here -->
            </div>
            <!-- Loading indicator -->
            <div id="loadingIndicator" class="hidden my-2 flex justify-start">
                <div class="w-8 h-8 rounded-full border-4 border-cyan-400 border-t-transparent animate-spin"></div>
            </div>
        </div>
    </div>

    <!-- Scroll to bottom button -->
    <button id="scrollToBottomBtn" class="fixed bottom-24 right-6 bg-cyan-600 hover:bg-cyan-700 text-white p-3 rounded-full shadow-lg transition-all hidden z-30">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3" />
        </svg>
    </button>

    <!-- Input Section -->
    <div class="w-full p-2 flex justify-center sticky bottom-0 bg-gray-950">
        <div class="w-full max-w-2xl flex items-center gap-2 bg-gray-800 rounded-full pr-2">
            <textarea id="userInput" class="flex-1 p-3 text-white bg-transparent outline-none resize-none overflow-hidden rounded-full h-12" placeholder="اكتب رسالتك هنا..." dir="rtl" readonly></textarea>
            <div class="flex space-x-2 space-x-reverse">
                <button id="summarizeBtn" class="w-auto px-4 py-2 bg-blue-500 hover:bg-blue-600 text-white rounded-full transition-colors whitespace-nowrap">تلخيص ✨</button>
                <button id="speakBtn" class="w-auto px-4 py-2 bg-purple-500 hover:bg-purple-600 text-white rounded-full transition-colors whitespace-nowrap">تحدث معي ✨</button>
                <button id="sendBtn" class="w-10 h-10 bg-cyan-500 hover:bg-cyan-600 rounded-full flex justify-center items-center transition-colors">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white" class="w-6 h-6 transform rotate-180">
                        <path d="M3.478 2.404a.75.75 0 011.06-.06l16.5 15a.75.75 0 010 1.06l-16.5 15a.75.75 0 01-1.06-.06.75.75 0 01-.06-1.06l15.47-14.5a.75.75 0 000-1.06l-15.47-14.5a.75.75 0 01.06-1.06z" />
                    </svg>
                </button>
            </div>
        </div>
    </div>

</body>
</html>
