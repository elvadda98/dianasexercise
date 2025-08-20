<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Practice Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 35px;
            text-align: center;
            position: relative;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="30" r="1.5" fill="rgba(255,255,255,0.1)"/><circle cx="40" cy="70" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="90" cy="80" r="2.5" fill="rgba(255,255,255,0.1)"/></svg>');
        }

        .header h1 {
            font-size: 2.8em;
            margin-bottom: 15px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.3em;
            opacity: 0.95;
            position: relative;
            z-index: 1;
            max-width: 600px;
            margin: 0 auto;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 15px;
            padding: 25px;
            background: #f8f9fa;
            flex-wrap: wrap;
            border-bottom: 1px solid #e9ecef;
        }

        .nav-btn {
            padding: 14px 28px;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            font-size: 17px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(71,118,230,0.35);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #555;
            border: 2px solid #e9ecef;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.12);
        }

        .game-section {
            display: none;
            padding: 35px;
            min-height: 450px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.6s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(25px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #eef2f8);
            padding: 28px;
            border-radius: 18px;
            margin-bottom: 30px;
            border: 2px solid #4776E6;
            box-shadow: 0 6px 20px rgba(71,118,230,0.18);
        }

        .word-bank h3 {
            color: #2c3e50;
            margin-bottom: 18px;
            text-align: center;
            font-size: 1.5em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 12px 22px;
            border-radius: 25px;
            font-weight: bold;
            font-size: 17px;
            box-shadow: 0 4px 12px rgba(71,118,230,0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-3px);
            box-shadow: 0 7px 18px rgba(71,118,230,0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 28px;
            border-radius: 18px;
            margin-bottom: 25px;
            border-left: 6px solid #4776E6;
            box-shadow: 0 6px 18px rgba(0,0,0,0.06);
        }

        .question h3 {
            color: #2c3e50;
            margin-bottom: 18px;
            font-size: 1.4em;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .question h3:before {
            content: "•";
            color: #4776E6;
            font-size: 1.8em;
        }

        .question p {
            font-size: 18px;
            line-height: 1.6;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 10px;
            padding: 10px 15px;
            font-size: 17px;
            margin: 0 6px;
            min-width: 140px;
            transition: border-color 0.3s ease;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .fill-blank:focus {
            outline: none;
            border-color: #4776E6;
            box-shadow: 0 0 0 3px rgba(71,118,230,0.2);
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 18px;
            margin-top: 18px;
        }

        .option {
            padding: 18px 22px;
            border: 2px solid #ddd;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
            font-size: 16px;
        }

        .option:hover {
            border-color: #4776E6;
            transform: translateY(-3px);
            box-shadow: 0 6px 18px rgba(71,118,230,0.15);
        }

        .option.selected {
            background: #4776E6;
            color: white;
            border-color: #4776E6;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 35px;
            margin-top: 25px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 20px;
            color: #2c3e50;
            font-size: 1.3em;
            padding-bottom: 10px;
            border-bottom: 2px solid #e9ecef;
        }

        .match-item {
            padding: 18px;
            margin: 10px 0;
            border: 2px solid #ddd;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
            font-size: 16px;
        }

        .match-item:hover {
            border-color: #4776E6;
            transform: translateY(-2px);
        }

        .match-item.selected {
            background: #f0f4ff;
            border-color: #4776E6;
            box-shadow: 0 4px 12px rgba(71,118,230,0.2);
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
            transform: scale(0.98);
        }

        .check-btn {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            border: none;
            padding: 18px 35px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            margin: 30px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 6px 20px rgba(71,118,230,0.3);
        }

        .check-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 9px 25px rgba(71,118,230,0.4);
        }

        .check-btn:active {
            transform: translateY(1px);
        }

        .feedback {
            margin-top: 20px;
            padding: 18px;
            border-radius: 12px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.4s ease;
            font-size: 17px;
        }

        @keyframes slideIn {
            from { transform: translateX(-25px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 25px;
            right: 25px;
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 18px 25px;
            border-radius: 30px;
            font-weight: bold;
            box-shadow: 0 6px 20px rgba(71,118,230,0.35);
            z-index: 1000;
            font-size: 18px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .score-number {
            font-size: 1.2em;
            background: white;
            color: #4776E6;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 25px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 220px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: inline-flex;
                width: auto;
            }
            
            .header h1 {
                font-size: 2.2em;
            }
            
            .header p {
                font-size: 1.1em;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span class="score-number" id="score">0</span>/20</div>
    
    <div class="container">
        <div class="header">
            <h1>📚 Vocabulary Builder</h1>
            <p>Practice and master these useful English words and phrases!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('fill-gaps')">Fill in the Gaps</button>
            <button class="nav-btn" onclick="showSection('matching')">Match Meanings</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Multiple Choice</button>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section active">
            <div class="word-bank">
                <h3>📝 Word Bank - Choose from these words:</h3>
                <div class="word-options">
                    <span class="word-option">crowded</span>
                    <span class="word-option">out of fashion</span>
                    <span class="word-option">stylish</span>
                    <span class="word-option">fashionable</span>
                    <span class="word-option">lack</span>
                    <span class="word-option">give up</span>
                    <span class="word-option">between</span>
                    <span class="word-option">shame</span>
                    <span class="word-option">close</span>
                    <span class="word-option">sick</span>
                    <span class="word-option">goals</span>
                    <span class="word-option">often</span>
                    <span class="word-option">relatives</span>
                </div>
            </div>

            <div id="questions-container">
                <!-- Questions will be dynamically inserted here -->
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
        </div>

        <!-- Matching Section with Shuffled Meanings -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 25px; color: #2c3e50;">Match the words with their meanings</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Words</h4>
                    <div id="words-column">
                        <!-- Words will be dynamically inserted here -->
                    </div>
                </div>
                <div class="match-column">
                    <h4>Meanings</h4>
                    <div id="meanings-column">
                        <!-- Meanings will be dynamically inserted here -->
                    </div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Question 1: What does "crowded" typically describe?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">An empty room</div>
                    <div class="option" onclick="selectOption(this, true)">A place with too many people</div>
                    <div class="option" onclick="selectOption(this, false)">A quiet space</div>
                    <div class="option" onclick="selectOption(this, false)">A remote location</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2: If something is "out of fashion", it is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Very trendy</div>
                    <div class="option" onclick="selectOption(this, false)">Brand new</div>
                    <div class="option" onclick="selectOption(this, true)">No longer popular</div>
                    <div class="option" onclick="selectOption(this, false)">Extremely valuable</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3: "Stylish" and "fashionable" are:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Opposites</div>
                    <div class="option" onclick="selectOption(this, true)">Similar in meaning</div>
                    <div class="option" onclick="selectOption(this, false)">Completely unrelated</div>
                    <div class="option" onclick="selectOption(this, false)">Technical terms</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4: When you "give up" on something, you:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Start a new project</div>
                    <div class="option" onclick="selectOption(this, false)">Work harder at it</div>
                    <div class="option" onclick="selectOption(this, true)">Stop trying</div>
                    <div class="option" onclick="selectOption(this, false)">Ask for help</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5: "Relatives" refers to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Close friends</div>
                    <div class="option" onclick="selectOption(this, false)">Neighbors</div>
                    <div class="option" onclick="selectOption(this, true)">Family members</div>
                    <div class="option" onclick="selectOption(this, false)">Colleagues</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Define the questions for the fill-in-the-blanks section
        const questions = [
            {
                id: 1,
                text: "The subway during rush hour is always extremely <input type='text' class='fill-blank' data-answer='crowded' placeholder='answer'>.",
                answer: "crowded"
            },
            {
                id: 2,
                text: "Bell-bottom pants were popular in the 70s but are now considered <input type='text' class='fill-blank' data-answer='out of fashion' placeholder='answer'>.",
                answer: "out of fashion"
            },
            {
                id: 3,
                text: "She always wears <input type='text' class='fill-blank' data-answer='stylish' placeholder='answer'> clothing that gets compliments.",
                answer: "stylish"
            },
            {
                id: 4,
                text: "Wide-leg jeans are very <input type='text' class='fill-blank' data-answer='fashionable' placeholder='answer'> this season.",
                answer: "fashionable"
            },
            {
                id: 5,
                text: "Many people in developing countries <input type='text' class='fill-blank' data-answer='lack' placeholder='answer'> access to clean water.",
                answer: "lack"
            },
            {
                id: 6,
                text: "Don't <input type='text' class='fill-blank' data-answer='give up' placeholder='answer'> on your dreams, no matter how difficult things get.",
                answer: "give up"
            },
            {
                id: 7,
                text: "The small town is located <input type='text' class='fill-blank' data-answer='between' placeholder='answer'> two large mountains.",
                answer: "between"
            },
            {
                id: 8,
                text: "It's a <input type='text' class='fill-blank' data-answer='shame' placeholder='answer'> that such a talented artist never received recognition.",
                answer: "shame"
            },
            {
                id: 9,
                text: "My grandparents live <input type='text' class='fill-blank' data-answer='close' placeholder='answer'> to us, just a few blocks away.",
                answer: "close"
            },
            {
                id: 10,
                text: "He called in to work because he was feeling <input type='text' class='fill-blank' data-answer='sick' placeholder='answer'>.",
                answer: "sick"
            },
            {
                id: 11,
                text: "Setting clear <input type='text' class='fill-blank' data-answer='goals' placeholder='answer'> is important for success.",
                answer: "goals"
            },
            {
                id: 12,
                text: "She <input type='text' class='fill-blank' data-answer='often' placeholder='answer'> goes for a run in the morning before work.",
                answer: "often"
            },
            {
                id: 13,
                text: "We're having a family reunion with all our <input type='text' class='fill-blank' data-answer='relatives' placeholder='answer'> this weekend.",
                answer: "relatives"
            }
        ];
        
        // Define the matching pairs
        const matchingPairs = [
            { word: "crowded", meaning: "Filled with too many people or things" },
            { word: "out of fashion", meaning: "No longer popular or trendy" },
            { word: "stylish", meaning: "Fashionably elegant and sophisticated" },
            { word: "fashionable", meaning: "Following current styles and trends" },
            { word: "lack", meaning: "The state of being without or not having enough" },
            { word: "give up", meaning: "To stop trying or quit doing something" },
            { word: "between", meaning: "In the space separating two things" },
            { word: "shame", meaning: "A painful feeling of humiliation or distress" },
            { word: "close", meaning: "Near in space, time, or relationship" },
            { word: "sick", meaning: "Affected by physical or mental illness" },
            { word: "goals", meaning: "Objectives or aims that one works to achieve" },
            { word: "often", meaning: "Frequently; many times" },
            { word: "relatives", meaning: "People connected by blood or marriage" }
        ];
        
        // Function to shuffle the questions array
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
        
        // Function to render the shuffled questions
        function renderQuestions() {
            const container = document.getElementById('questions-container');
            container.innerHTML = '';
            
            const shuffledQuestions = shuffleArray([...questions]);
            
            shuffledQuestions.forEach((question, index) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                questionDiv.innerHTML = `
                    <h3>Question ${index + 1}:</h3>
                    <p>${question.text}</p>
                    <div class="feedback" id="feedback-${question.id}" style="display: none;"></div>
                `;
                container.appendChild(questionDiv);
            });
        }
        
        // Function to render the matching section with shuffled meanings
        function renderMatchingSection() {
            const wordsColumn = document.getElementById('words-column');
            const meaningsColumn = document.getElementById('meanings-column');
            
            wordsColumn.innerHTML = '';
            meaningsColumn.innerHTML = '';
            
            // Shuffle the matching pairs
            const shuffledPairs = shuffleArray([...matchingPairs]);
            
            // Create word elements
            shuffledPairs.forEach(pair => {
                const wordElement = document.createElement('div');
                wordElement.className = 'match-item';
                wordElement.dataset.word = pair.word;
                wordElement.textContent = pair.word;
                wordElement.onclick = function() { selectMatch(this); };
                wordsColumn.appendChild(wordElement);
            });
            
            // Shuffle the meanings separately
            const shuffledMeanings = shuffleArray([...matchingPairs.map(pair => pair.meaning)]);
            
            // Create meaning elements
            shuffledMeanings.forEach(meaning => {
                const meaningElement = document.createElement('div');
                meaningElement.className = 'match-item';
                meaningElement.dataset.meaning = matchingPairs.find(pair => pair.meaning === meaning).word;
                meaningElement.textContent = meaning;
                meaningElement.onclick = function() { selectMatch(this); };
                meaningsColumn.appendChild(meaningElement);
            });
            
            // Reset matching state
            matchedPairs = [];
            selectedWord = null;
            selectedMeaning = null;
            document.getElementById('matching-feedback').style.display = 'none';
        }
        
        // Call the function to render questions when the page loads
        window.onload = function() {
            renderQuestions();
            renderMatchingSection();
        };

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If it's the fill-gaps section, reshuffle the questions
            if (sectionId === 'fill-gaps') {
                renderQuestions();
            }
            
            // If it's the matching section, reshuffle the meanings
            if (sectionId === 'matching') {
                renderMatchingSection();
            }
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
            });
            
            // Show feedback for each question
            questions.forEach(question => {
                const feedback = document.getElementById(`feedback-${question.id}`);
                const blank = document.querySelector(`.fill-blank[data-answer="${question.answer}"]`);
                
                if (blank && blank.classList.contains('correct')) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${question.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Correct match!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === 13) {
                    feedback.textContent = '🎉 Congratulations! You matched all terms correctly!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Incorrect match. Try again.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Correct!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Incorrect. Try again.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            updateScore();
        }

        function updateScore() {
            // Calculate score for fill-in-the-blank
            let fillScore = 0;
            document.querySelectorAll('.fill-blank.correct').forEach(blank => {
                if (!blank.classList.contains('counted')) {
                    fillScore++;
                    blank.classList.add('counted');
                }
            });
            
            // Calculate score for matching (each pair is 1 point)
            const matchScore = matchedPairs.length;
            
            // Calculate score for multiple choice (each correct is 1 point)
            let mcScore = 0;
            document.querySelectorAll('.option.correct.selected').forEach(opt => {
                mcScore++;
            });
            
            // Total score
            score = fillScore + matchScore + mcScore;
            document.getElementById('score').textContent = score;
        }
    </script>
</body>
</html>
