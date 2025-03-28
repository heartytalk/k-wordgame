<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>한글 메모리 카드 게임</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; margin: 0; }
        .grid { display: grid; gap: 10px; justify-content: center; align-items: center; margin-top: 20px; }
        .card { 
            width: 180px; 
            height: 180px; 
            font-size: 72px; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            background: lightblue; 
            cursor: pointer; 
            border-radius: 5px; 
            box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.2);
        }
        .hidden { background: darkgray; }
        .footer { margin-top: 20px; font-size: 14px; color: gray; text-align: center; }
    </style>
</head>
<body>
    <h1>한글 메모리 카드 게임</h1>
    <label for="size">게임 크기 선택:</label>
    <select id="size" onchange="setupGame()">
        <option value="2">2x2</option>
        <option value="3">3x3</option>
        <option value="4">4x4</option>
        <option value="5">5x5</option>
        <option value="6">6x6</option>
    </select>
    <div id="gameBoard" class="grid"></div>
    <p id="message"></p>
    
    <script>
        const hangulChars = ['ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ', 'ㅏ', 'ㅑ', 'ㅓ', 'ㅕ', 'ㅗ', 'ㅛ', 'ㅜ', 'ㅠ', 'ㅡ', 'ㅣ'];
        let boardSize = 3;
        let cards = [];
        let firstCard, secondCard;
        let lockBoard = false;
        let matchedPairs = 0;

        function setupGame() {
            boardSize = parseInt(document.getElementById('size').value);
            let totalCards = boardSize * boardSize;
            let gameBoard = document.getElementById('gameBoard');
            let message = document.getElementById('message');
            gameBoard.innerHTML = '';
            message.textContent = '';
            gameBoard.style.gridTemplateColumns = `repeat(${boardSize}, 1fr)`;
            
            let selectedChars = hangulChars.sort(() => 0.5 - Math.random()).slice(0, Math.floor(totalCards / 2));
            let cardValues = [...selectedChars, ...selectedChars].sort(() => Math.random() - 0.5);
            
            if (cardValues.length < totalCards) {
                let extraChar = hangulChars.find(c => !selectedChars.includes(c));
                cardValues.push(extraChar);
            }
            
            cards = [];
            matchedPairs = 0;
            cardValues.forEach((char, index) => {
                let card = document.createElement('div');
                card.classList.add('card', 'hidden');
                card.dataset.value = char;
                card.addEventListener('click', flipCard);
                gameBoard.appendChild(card);
                cards.push(card);
            });
        }

        function flipCard() {
            if (lockBoard || this === firstCard || !this.classList.contains('hidden')) return;
            
            this.textContent = this.dataset.value;
            this.classList.remove('hidden');
            
            if (!firstCard) {
                firstCard = this;
                return;
            }
            
            secondCard = this;
            lockBoard = true;
            
            if (firstCard.dataset.value === secondCard.dataset.value) {
                matchedPairs++;
                firstCard = secondCard = null;
                lockBoard = false;
                
                if (matchedPairs === Math.floor((boardSize * boardSize) / 2)) {
                    document.getElementById('message').textContent = '축하합니다! 게임을 완료했습니다!';
                }
            } else {
                setTimeout(() => {
                    firstCard.textContent = '';
                    secondCard.textContent = '';
                    firstCard.classList.add('hidden');
                    secondCard.classList.add('hidden');
                    firstCard = secondCard = null;
                    lockBoard = false;
                }, 1000);
            }
        }

        setupGame();
    </script>
    
    <div class="footer">
        <p>하트선생님이 언어치료수업을 위해 만들었어요.</p>
        <p>@Heartytalk</p>
        <p>tjdah0420@naver.com</p>
        <p><a href="https://blog.naver.com/mindcarelog" target="_blank">https://blog.naver.com/mindcarelog</a></p>
    </div>
</body>
</html>
