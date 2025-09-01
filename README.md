<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>العتصم AI</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #f6fdfa; /* لون خلفية أخضر فاتح جداً */
            color: #2c523e; /* لون نص أخضر داكن */
        }
        .header-bg {
            background-color: #38a169; /* لون أخضر جميل للرأس */
        }
        .ai-card {
            background-color: #e6f6ec; /* لون بطاقة أخضر فاتح جداً */
            border-color: #68d391; /* حدود خضراء ناعمة */
        }
        .game-card {
            background-color: #e6f6ec;
            border-color: #68d391;
            @apply rounded-3xl p-6 shadow-2xl;
        }
        .game-button {
            @apply bg-emerald-600 text-white font-bold py-2 px-6 rounded-full shadow-lg hover:bg-emerald-700 transition-colors duration-300;
        }
        .gallery-item {
            @apply rounded-2xl shadow-lg hover:shadow-2xl transition-transform duration-300 transform hover:scale-105;
        }
        .dark {
            background-color: #121c16;
            color: #e6f6ec;
        }
        .dark .ai-card, .dark .game-card {
            background-color: #1a362f;
            border-color: #064e3b;
        }
        .dark .header-bg {
            background-color: #047857;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100">

    <!-- شريط الرأس والبحث -->
    <header class="header-bg text-white shadow-lg sticky top-0 z-50">
        <div class="container mx-auto px-4 py-4 flex flex-col md:flex-row justify-between items-center">
            <h1 class="text-3xl font-extrabold tracking-widest">AI المعتصم</h1>
            <div class="flex-grow flex items-center justify-center mt-4 md:mt-0 md:mr-4">
                <input id="ai-input" type="text" placeholder="اكتب استفسارك هنا..." class="w-full max-w-lg p-2 rounded-l-full text-gray-800 focus:outline-none focus:ring-2 focus:ring-emerald-500">
                <button id="ai-search-btn" class="bg-emerald-700 text-white px-6 py-2 rounded-r-full hover:bg-emerald-800 transition-colors duration-300">بحث</button>
            </div>
            <button id="modeToggle" class="text-3xl hover:text-emerald-300 transition-colors duration-300 mt-4 md:mt-0">
                ☀️
            </button>
        </div>
    </header>

    <main class="container mx-auto px-4 py-8 space-y-12">
        <!-- قسم نتائج الذكاء الاصطناعي -->
        <section id="ai-results" class="ai-card rounded-3xl shadow-xl p-6 md:p-8 hidden">
            <h2 class="text-2xl font-bold mb-4">نتائج البحث</h2>
            <div id="ai-content" class="space-y-4">
                <!-- المحتوى سيتم إضافته هنا بواسطة JavaScript -->
            </div>
        </section>

        <!-- قسم المعرض -->
        <section id="gallery" class="ai-card rounded-3xl shadow-xl p-6 md:p-8">
            <h2 class="text-2xl font-bold text-center mb-6">معرض صور وفيديوهات</h2>
            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
                <!-- الصور التي تم رفعها -->
                <img src="https://storage.googleapis.com/uploaded_files_and_media/ChatGPT%20Image%20Aug%2029%2C%202025%2C%2007_12_23%20PM.jpg" alt="صورة مجردة" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Gemini_Generated_Image_7w9fi47w9fi47w9f.jpg" alt="رسم تخطيطي لوجه حزين" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Gemini_Generated_Image_iyshz3iyshz3iysh.jpg" alt="لوحة فنية لطبيعة" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Gemini_Generated_Image_e1zivde1zivde1zi.jpg" alt="سيارة سباق في مدينة ليلية" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Gemini_Generated_Image_8hdlkb8hdlkb8hdl.jpg" alt="شخص يركب الأمواج" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Screenshot%202025-08-02%20005925.jpg" alt="كوكب ملون في الفضاء" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Gemini_Generated_Image_h7d765h7d765h7d7.jpg" alt="سديم في الفضاء" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Gemini_Generated_Image_gqed9xgqed9xgqed.jpg" alt="برج إيفل في الليل" class="gallery-item w-full h-48 object-cover">
                <img src="https://storage.googleapis.com/uploaded_files_and_media/Gemini_Generated_Image_winu2mwinu2mwinu.jpg" alt="مدينة ليلية مضيئة" class="gallery-item w-full h-48 object-cover">
                <!-- صور AI إضافية -->
                <img src="https://images.unsplash.com/photo-1517400583424-9407384a781c?q=80&w=1770&auto=format&fit=crop" alt="غابة استوائية" class="gallery-item w-full h-48 object-cover">
                <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?q=80&w=1770&auto=format&fit=crop" alt="شلال منعش" class="gallery-item w-full h-48 object-cover">
                <img src="https://images.unsplash.com/photo-1549495763-7c010c73de29?q=80&w=1770&auto=format&fit=crop" alt="نهر يتدفق بين الصخور" class="gallery-item w-full h-48 object-cover">
                <!-- فيديوهات -->
                <iframe class="gallery-item w-full h-48" src="https://www.youtube.com/embed/S2gX66D5x6c" title="Forests" allowfullscreen></iframe>
                <iframe class="gallery-item w-full h-48" src="https://www.youtube.com/embed/6T66l7u-U6s" title="Jungle" allowfullscreen></iframe>
                <iframe class="gallery-item w-full h-48" src="https://www.youtube.com/embed/8yU9D490UeY" title="Desert" allowfullscreen></iframe>
            </div>
        </section>

        <!-- قسم الألعاب -->
        <section id="games" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            <!-- لعبة البستنة -->
            <div id="game-garden" class="game-card">
                <h2 class="text-xl font-bold text-center mb-4">لعبة البستنة 🪴</h2>
                <div class="flex flex-col items-center space-y-4">
                    <img id="garden-plant" src="https://www.svgrepo.com/show/368817/plant-sprout.svg" alt="نبتة صغيرة" class="w-20 h-20">
                    <button id="garden-water-btn" class="game-button">اسقِ النبتة!</button>
                    <p id="garden-message" class="text-lg font-bold"></p>
                </div>
            </div>

            <!-- لعبة متاهة النحلة -->
            <div id="game-maze" class="game-card">
                <h2 class="text-xl font-bold text-center mb-4">متاهة النحلة 🐝</h2>
                <div class="flex flex-col items-center space-y-4">
                    <div id="maze-board" class="grid w-64 h-64 border-2 border-green-600 rounded-lg"></div>
                    <p id="maze-message" class="text-lg font-bold"></p>
                </div>
            </div>

            <!-- لعبة الألغاز -->
            <div id="game-puzzle" class="game-card">
                <h2 class="text-xl font-bold text-center mb-4">لغز الطبيعة 🧩</h2>
                <div class="flex flex-col items-center space-y-4">
                    <div id="puzzle-board" class="grid grid-cols-3 grid-rows-3 w-64 h-64 border-2 border-green-600 rounded-lg"></div>
                    <button id="puzzle-reset-btn" class="game-button">ابدأ من جديد</button>
                    <p id="puzzle-message" class="text-lg font-bold"></p>
                </div>
            </div>
        </section>

    </main>

    <!-- تذييل الصفحة -->
    <footer class="header-bg text-white text-center py-6 mt-12 rounded-t-3xl shadow-lg">
        <div class="flex justify-center space-x-6 mb-4">
            <a href="https://www.youtube.com/@My_VisualMoments" target="_blank" class="hover:opacity-75 transition-opacity">
                <img src="https://www.svgrepo.com/show/506487/youtube.svg" alt="يوتيوب" class="w-10 h-10">
            </a>
            <a href="https://www.instagram.com/al_muthman/" target="_blank" class="hover:opacity-75 transition-opacity">
                <img src="https://www.svgrepo.com/show/452144/instagram-1.svg" alt="إنستغرام" class="w-10 h-10">
            </a>
        </div>
        <p class="text-lg">&copy; 2025 AI المعتصم - جميع الحقوق محفوظة</p>
    </footer>

    <script>
        // تبديل الوضع (داكن/فاتح)
        const modeToggle = document.getElementById('modeToggle');
        modeToggle.onclick = function() {
            document.body.classList.toggle('dark');
            modeToggle.innerHTML = document.body.classList.contains('dark') ? '🌙' : '☀️';
        }

        // منطق شريط AI المعتصم
        const aiInput = document.getElementById('ai-input');
        const aiSearchBtn = document.getElementById('ai-search-btn');
        const aiResultsSection = document.getElementById('ai-results');
        const aiContent = document.getElementById('ai-content');

        aiSearchBtn.addEventListener('click', () => {
            const query = aiInput.value.trim();
            if (query === '') {
                aiResultsSection.classList.add('hidden');
                return;
            }

            aiResultsSection.classList.remove('hidden');
            aiContent.innerHTML = `<p class="text-center text-lg">جارٍ البحث عن "${query}"...</p>`;

            setTimeout(() => {
                aiContent.innerHTML = '';
                let resultText = '';

                if (query.includes('صورة') || query.includes('صور') || query.includes('صور الطبيعة')) {
                    resultText = 'إليك بعض الصور التي تم إنشاؤها بالذكاء الاصطناعي بناءً على طلبك.';
                    aiContent.innerHTML += `<div class="grid grid-cols-2 md:grid-cols-3 gap-4">
                        <img src="https://images.unsplash.com/photo-1549495763-7c010c73de29?q=80&w=600&auto=format&fit=crop" class="rounded-lg w-full h-auto" alt="صورة AI">
                        <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?q=80&w=600&auto=format&fit=crop" class="rounded-lg w-full h-auto" alt="صورة AI">
                        <img src="https://images.unsplash.com/photo-1517400583424-9407384a781c?q=80&w=600&auto=format&fit=crop" class="rounded-lg w-full h-auto" alt="صورة AI">
                    </div>`;
                } else if (query.includes('فيديو') || query.includes('فيديوهات')) {
                    resultText = 'إليك مقطع فيديو بناءً على طلبك.';
                    aiContent.innerHTML += `<iframe class="w-full h-64 rounded-lg" src="https://www.youtube.com/embed/S2gX66D5x6c" title="AI Video" frameborder="0" allowfullscreen></iframe>`;
                } else if (query.includes('مستند') || query.includes('وثيقة')) {
                    resultText = 'تم إنشاء مستند حول موضوع طلبك. <br><p class="text-sm text-gray-500 mt-2">ملاحظة: هذا مجرد مثال توضيحي.</p>';
                } else {
                    resultText = 'لا يمكن العثور على نتائج مطابقة لطلبك.';
                }

                aiContent.innerHTML = `<p class="text-lg mb-4">${resultText}</p>` + aiContent.innerHTML;
            }, 1500);
        });

        // منطق لعبة البستنة
        const gardenPlant = document.getElementById('garden-plant');
        const gardenWaterBtn = document.getElementById('garden-water-btn');
        const gardenMessage = document.getElementById('garden-message');
        let waterCount = 0;
        const plantStages = [
            "https://www.svgrepo.com/show/368817/plant-sprout.svg", // مرحلة 0
            "https://www.svgrepo.com/show/368818/plant-pot.svg", // مرحلة 1
            "https://www.svgrepo.com/show/368819/plant-growing.svg", // مرحلة 2
            "https://www.svgrepo.com/show/368820/plant-blooming.svg" // مرحلة 3
        ];
        gardenWaterBtn.addEventListener('click', () => {
            if (waterCount < plantStages.length - 1) {
                waterCount++;
                gardenPlant.src = plantStages[waterCount];
                gardenMessage.textContent = `النبتة تكبر! تحتاج إلى ${plantStages.length - 1 - waterCount} ريّة أخرى.`;
            } else {
                gardenMessage.textContent = 'أحسنت! النبتة نمت بالكامل!';
                gardenWaterBtn.disabled = true;
            }
        });

        // منطق لعبة متاهة النحلة
        const mazeBoard = document.getElementById('maze-board');
        const mazeMessage = document.getElementById('maze-message');
        const mazeSize = 10;
        let beePosition = { x: 0, y: 0 };
        const flowerPosition = { x: mazeSize - 1, y: mazeSize - 1 };

        function createMaze() {
            mazeBoard.innerHTML = '';
            mazeBoard.style.gridTemplateColumns = `repeat(${mazeSize}, 1fr)`;
            for (let i = 0; i < mazeSize * mazeSize; i++) {
                const cell = document.createElement('div');
                cell.classList.add('w-full', 'h-full', 'border', 'border-gray-300', 'flex', 'items-center', 'justify-center');
                mazeBoard.appendChild(cell);
            }
            drawBee();
            drawFlower();
            mazeMessage.textContent = "استخدم الأسهم لتحريك النحلة إلى الزهرة!";
        }

        function drawBee() {
            const cell = mazeBoard.children[beePosition.y * mazeSize + beePosition.x];
            cell.innerHTML = '🐝';
        }
        function drawFlower() {
            const cell = mazeBoard.children[flowerPosition.y * mazeSize + flowerPosition.x];
            cell.innerHTML = '🌸';
        }

        document.addEventListener('keydown', (e) => {
            let newX = beePosition.x;
            let newY = beePosition.y;

            if (e.key === 'ArrowUp') newY--;
            if (e.key === 'ArrowDown') newY++;
            if (e.key === 'ArrowLeft') newX--;
            if (e.key === 'ArrowRight') newX++;

            if (newX >= 0 && newX < mazeSize && newY >= 0 && newY < mazeSize) {
                // مسح النحلة القديمة
                mazeBoard.children[beePosition.y * mazeSize + beePosition.x].innerHTML = '';
                // تحديث الموضع
                beePosition.x = newX;
                beePosition.y = newY;
                // رسم النحلة الجديدة
                drawBee();
                
                if (beePosition.x === flowerPosition.x && beePosition.y === flowerPosition.y) {
                    mazeMessage.textContent = '🎉 لقد وصلت! أحسنت! 🎉';
                }
            }
        });

        // منطق لعبة الألغاز
        const puzzleBoard = document.getElementById('puzzle-board');
        const puzzleResetBtn = document.getElementById('puzzle-reset-btn');
        const puzzleMessage = document.getElementById('puzzle-message');
        const puzzleImage = "https://images.unsplash.com/photo-1490730114066-88d40786522c?q=80&w=600&auto=format&fit=crop";
        let tiles = [];

        function createPuzzle() {
            puzzleBoard.innerHTML = '';
            tiles = Array.from({ length: 9 }, (_, i) => i + 1);
            shuffleTiles(tiles);
            tiles.forEach((tile, index) => {
                const tileElement = document.createElement('div');
                tileElement.classList.add('border-2', 'border-green-600', 'cursor-pointer', 'transition-all', 'duration-300');
                tileElement.style.backgroundImage = `url(${puzzleImage})`;
                tileElement.style.backgroundSize = '300% 300%';
                tileElement.style.backgroundPosition = `${((tile - 1) % 3) * 50}% ${Math.floor((tile - 1) / 3) * 50}%`;
                tileElement.dataset.index = tile;
                tileElement.addEventListener('click', () => moveTile(index));
                puzzleBoard.appendChild(tileElement);
            });
            puzzleMessage.textContent = "رتّب الأجزاء لتكوّن الصورة!";
        }

        function shuffleTiles(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        function moveTile(index) {
            const emptyIndex = tiles.indexOf(9);
            const isValidMove =
                (Math.abs(Math.floor(index / 3) - Math.floor(emptyIndex / 3)) === 1 && Math.floor(index / 3) === Math.floor(emptyIndex / 3)) ||
                (Math.abs(Math.floor(index / 3) - Math.floor(emptyIndex / 3)) === 0 && Math.abs((index % 3) - (emptyIndex % 3)) === 1);
            
            if (isValidMove) {
                [tiles[index], tiles[emptyIndex]] = [tiles[emptyIndex], tiles[index]];
                updatePuzzleBoard();
                if (isSolved()) {
                    puzzleMessage.textContent = '🎉 أحسنت! لقد أكملت اللغز! 🎉';
                }
            }
        }
        
        function updatePuzzleBoard() {
            tiles.forEach((tile, index) => {
                const tileElement = puzzleBoard.children[index];
                if (tile === 9) {
                    tileElement.style.backgroundImage = 'none';
                    tileElement.style.backgroundColor = '#d1d5db';
                } else {
                    tileElement.style.backgroundImage = `url(${puzzleImage})`;
                    tileElement.style.backgroundPosition = `${((tile - 1) % 3) * 50}% ${Math.floor((tile - 1) / 3) * 50}%`;
                }
            });
        }

        function isSolved() {
            return tiles.every((tile, index) => tile === index + 1);
        }
        
        puzzleResetBtn.addEventListener('click', createPuzzle);

        // تشغيل الألعاب عند تحميل الصفحة
        window.onload = function() {
            createMaze();
            createPuzzle();
        }
    </script>
</body>
</html>
