<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HSK 6 Word Quiz</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        /* Custom font for Chinese characters */
        .hanzi {
            font-family: 'Noto Sans SC', sans-serif;
        }
        /* Simple transition for better user experience */
        .transition-all {
            transition: all 0.3s ease-in-out;
        }
    </style>
</head>
<body class="bg-slate-100 dark:bg-slate-900 text-slate-800 dark:text-slate-200 flex items-center justify-center min-h-screen p-4">

    <div class="w-full max-w-2xl mx-auto bg-white dark:bg-slate-800 rounded-2xl shadow-2xl p-6 sm:p-8 text-center">
        
        <header class="mb-8">
            <h1 class="text-2xl sm:text-3xl font-bold text-sky-600 dark:text-sky-400">Тест по словам HSK 6</h1>
            <p class="text-slate-500 dark:text-slate-400 mt-2">Выберите правильный перевод для слова.</p>
        </header>

        <!-- Character Display -->
        <div id="character-container" class="mb-6">
            <div id="character-display" class="hanzi text-7xl sm:text-9xl font-bold text-slate-900 dark:text-white mb-2 h-28 sm:h-36 flex items-center justify-center"></div>
            <div id="pinyin-display" class="text-2xl text-slate-500 dark:text-slate-400 h-8"></div>
        </div>

        <!-- Options Container -->
        <div id="options-container" class="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
            <!-- Options will be dynamically inserted here -->
        </div>

        <!-- Result Message -->
        <div id="result-message" class="h-8 mb-4 text-xl font-semibold"></div>

        <!-- Next Word Button -->
        <button id="next-word-button" class="w-full sm:w-auto px-8 py-3 bg-sky-600 text-white font-semibold rounded-lg shadow-md hover:bg-sky-700 focus:outline-none focus:ring-2 focus:ring-sky-400 focus:ring-opacity-75 transition-all disabled:bg-slate-400 dark:disabled:bg-slate-600" disabled>
            Следующее слово
        </button>

    </div>

    <script>
        // A sample of HSK 6 words. A full list would have 2500 words.
        const wordList = [
            { hanzi: '爱不释手', pinyin: 'ài bù shì shǒu', russian: 'не выпускать из рук' },
            { hanzi: '安居乐业', pinyin: 'ān jū lè yè', russian: 'жить в мире и благоденствии' },
            { hanzi: '拔苗助长', pinyin: 'bá miáo zhù zhǎng', russian: 'тянуть ростки, чтобы помочь им расти (оказывать медвежью услугу)' },
            { hanzi: '半途而废', pinyin: 'bàn tú ér fèi', russian: 'бросить на полпути' },
            { hanzi: '包罗万象', pinyin: 'bāo luó wàn xiàng', russian: 'обнимать всё, содержать всё' },
            { hanzi: '饱经沧桑', pinyin: 'bǎo jīng cāng sāng', russian: 'претерпеть превратности судьбы' },
            { hanzi: '博大精深', pinyin: 'bó dà jīng shēn', russian: 'широкий и глубокий (о знаниях)' },
            { hanzi: '层出不穷', pinyin: 'céng chū bù qióng', russian: 'появляться одно за другим' },
            { hanzi: '称心如意', pinyin: 'chèn xīn rú yì', russian: 'полностью соответствовать желанию' },
            { hanzi: '川流不息', pinyin: 'chuān liú bù xī', russian: 'беспрерывный поток' },
            { hanzi: '从容不迫', pinyin: 'cóng róng bù pò', russian: 'спокойно, не торопясь' },
            { hanzi: '得意忘形', pinyin: 'dé yì wàng xíng', russian: 'потерять голову от успеха' },
            { hanzi: '废寝忘食', pinyin: 'fèi qǐn wàng shí', russian: 'забыть про сон и еду' },
            { hanzi: '风驰电掣', pinyin: 'fēng chí diàn chè', russian: 'мчаться как ветер' },
            { hanzi: '根深蒂固', pinyin: 'gēn shēn dì gù', russian: 'глубоко укоренившийся' },
            { hanzi: '津津有味', pinyin: 'jīn jīn yǒu wèi', russian: 'с большим интересом, с увлечением' },
            { hanzi: '精打细算', pinyin: 'jīng dǎ xì suàn', russian: 'тщательно подсчитывать' },
            { hanzi: '兢兢业业', pinyin: 'jīng jīng yè yè', russian: 'добросовестно, усердно' },
            { hanzi: '举世闻名', pinyin: 'jǔ shì wén míng', russian: 'всемирно известный' },
            { hanzi: '刻不容缓', pinyn: 'kè bù róng huǎn', russian: 'не допускать ни малейшего промедления' },
            { hanzi: '理所当然', pinyin: 'lǐ suǒ dāng rán', russian: 'само собой разумеется' },
            { hanzi: '络绎不绝', pinyin: 'luò yì bù jué', russian: 'непрерывной вереницей' },
            { hanzi: '迫不及待', pinyin: 'pò bù jí dài', russian: 'нетерпеливый, в нетерпении' },
            { hanzi: '齐心协力', pinyin: 'qí xīn xié lì', russian: 'общими усилиями' },
            { hanzi: '潜移默化', pinyin: 'qián yí mò huà', russian: 'незаметно влиять, перевоспитывать' },
            { hanzi: '全心全意', pinyin: 'quán xīn quán yì', russian: 'всем сердцем и всеми помыслами' },
            { hanzi: '无微不至', pinyin: 'wú wēi bù zhì', russian: 'с исключительной заботой' },
            { hanzi: '喜闻乐见', pinyin: 'xǐ wén lè jiàn', russian: 'пользоваться популярностью' },
            { hanzi: '循序渐进', pinyin: 'xún xù jiàn jìn', russian: 'постепенно и планомерно' },
            { hanzi: '一帆风顺', pinyin: 'yī fān fēng shùn', russian: 'попутного ветра' },
            { hanzi: '一举两得', pinyin: 'yī jǔ liǎng dé', russian: 'одним махом убить двух зайцев' },
            { hanzi: '再接再厉', pinyin: 'zài jiē zài lì', russian: 'продолжать с новыми силами' },
            { hanzi: '朝气蓬勃', pinyin: 'zhāo qì péng bó', russian: 'полный жизненных сил' },
            { hanzi: '众所周知', pinyin: 'zhòng suǒ zhōu zhī', russian: 'общеизвестно' },
            { hanzi: '莫名其妙', pinyin: 'mò míng qí miào', russian: 'непостижимый, необъяснимый' },
            { hanzi: '画蛇添足', pinyin: 'huà shé tiān zú', russian: 'нарисовав змею, пририсовать ей ноги (сделать лишнее)' },
            { hanzi: '见多识广', pinyin: 'jiàn duō shí guǎng', russian: 'многое повидать и узнать' },
            { hanzi: '全力以赴', pinyin: 'quán lì yǐ fù', russian: 'всеми силами' },
            { hanzi: '实事求是', pinyin: 'shí shì qiú shì', russian: 'основываться на фактах' },
            { hanzi: '讨价还价', pinyin: 'tǎo jià huán jià', russian: 'торговаться' }
        ];

        // DOM Elements
        const characterDisplay = document.getElementById('character-display');
        const pinyinDisplay = document.getElementById('pinyin-display');
        const optionsContainer = document.getElementById('options-container');
        const resultMessage = document.getElementById('result-message');
        const nextWordButton = document.getElementById('next-word-button');

        let currentWord = null;
        let correctOptionButton = null;

        // --- Game Logic ---

        /**
         * Shuffles an array in place.
         * @param {Array} array The array to shuffle.
         */
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        /**
         * Loads a new word and sets up the quiz.
         */
        function loadNewWord() {
            // 1. Reset UI
            resultMessage.textContent = '';
            resultMessage.className = 'h-8 mb-4 text-xl font-semibold';
            pinyinDisplay.textContent = '';
            optionsContainer.innerHTML = '';
            nextWordButton.disabled = true;

            // 2. Pick a new word randomly
            const wordIndex = Math.floor(Math.random() * wordList.length);
            currentWord = wordList[wordIndex];

            // 3. Display the character
            characterDisplay.textContent = currentWord.hanzi;

            // 4. Generate options (1 correct, 3 incorrect)
            const options = [currentWord.russian];
            const distractors = new Set();
            while (distractors.size < 3) {
                const randomWordIndex = Math.floor(Math.random() * wordList.length);
                if (randomWordIndex !== wordIndex) {
                    distractors.add(wordList[randomWordIndex].russian);
                }
            }
            options.push(...distractors);
            shuffleArray(options);

            // 5. Create and display option buttons
            options.forEach(optionText => {
                const button = document.createElement('button');
                button.textContent = optionText;
                button.className = 'w-full p-4 bg-slate-200 dark:bg-slate-700 rounded-lg text-left hover:bg-sky-100 dark:hover:bg-sky-900 transition-all';
                button.onclick = () => checkAnswer(optionText, button);
                optionsContainer.appendChild(button);

                if (optionText === currentWord.russian) {
                    correctOptionButton = button;
                }
            });
        }

        /**
         * Checks the user's selected answer.
         * @param {string} selectedTranslation The Russian translation selected by the user.
         * @param {HTMLButtonElement} selectedButton The button element that was clicked.
         */
        function checkAnswer(selectedTranslation, selectedButton) {
            // Disable all buttons after an answer is chosen
            const buttons = optionsContainer.getElementsByTagName('button');
            for (let btn of buttons) {
                btn.disabled = true;
                btn.classList.remove('hover:bg-sky-100', 'dark:hover:bg-sky-900');
            }
            
            // Show pinyin
            pinyinDisplay.textContent = currentWord.pinyin;

            // Check if the answer is correct
            if (selectedTranslation === currentWord.russian) {
                resultMessage.textContent = 'Правильно!';
                resultMessage.classList.add('text-green-500');
                selectedButton.className = 'w-full p-4 bg-green-500/20 dark:bg-green-500/30 text-green-800 dark:text-green-300 border-2 border-green-500 rounded-lg text-left font-semibold';
            } else {
                resultMessage.textContent = 'Неправильно!';
                resultMessage.classList.add('text-red-500');
                selectedButton.className = 'w-full p-4 bg-red-500/20 dark:bg-red-500/30 text-red-800 dark:text-red-300 border-2 border-red-500 rounded-lg text-left';
                // Also highlight the correct answer
                correctOptionButton.className = 'w-full p-4 bg-green-500/20 dark:bg-green-500/30 text-green-800 dark:text-green-300 border-2 border-green-500 rounded-lg text-left font-semibold';
            }

            // Enable the "Next Word" button
            nextWordButton.disabled = false;
        }

        // --- Event Listeners ---
        nextWordButton.addEventListener('click', loadNewWord);

        // --- Initial Load ---
        window.onload = () => {
            loadNewWord();
        };
    </script>
</body>
</html>
