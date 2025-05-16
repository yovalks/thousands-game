# thousands-game
Strategic card game
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Thousands Card Game</title>

    <style>

        body { font-family: Arial, sans-serif; background-color: #f4f4f9; margin: 0; padding: 20px; }

        #game-board { display: flex; gap: 20px; }

        #player-hand, #discard-pile, #draw-pile { border: 1px solid #ccc; padding: 10px; border-radius: 5px; }

        button { margin-top: 10px; }

        .card { display: inline-block; margin: 5px; padding: 5px 10px; background-color: #fff; border: 1px solid #aaa; border-radius: 5px; }

        #log { margin-top: 20px; height: 150px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; }

    </style>

</head>

<body>



<h2>Thousands Card Game</h2>



<div id="game-board">

    <div id="player-hand">

        <h3>Your Hand:</h3>

        <div id="hand-cards"></div>

    </div>

    <div id="discard-pile">

        <h3>Discard Pile:</h3>

        <div id="discard-cards"></div>

    </div>

    <div id="draw-pile">

        <h3>Draw Pile:</h3>

        <button onclick="drawCard()">Draw Card</button>

    </div>

</div>



<div id="log"><strong>Game Log:</strong></div>



<button onclick="pickupPile()">Pick Up Discard</button>

<button onclick="playCards()">Play Cards</button>

<button onclick="discardCard()">Discard Card</button>

<button onclick="callLastCard()">Call Last Card</button>



<script>

    // Initialize Game Logic

    const suits = ['hearts', 'diamonds', 'clubs', 'spades'];

    const ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];

    const specialCards = ['Joker'];

    const players = [{ name: 'You', hand: [], score: 0 }, { name: 'Bot 1', hand: [], score: 0 }, { name: 'Bot 2', hand: [], score: 0 }];

    let discardPile = [];

    let drawPile = [];



    function createDeck() {

        const deck = [];

        for (let i = 0; i < 2; i++) {

            suits.forEach(suit => {

                ranks.forEach(rank => {

                    deck.push({ rank, suit, points: getCardValue(rank, suit) });

                });

            });

            specialCards.forEach(() => {

                deck.push({ rank: 'Joker', suit: null, points: 20 });

            });

        }

        return deck;

    }



    function getCardValue(rank, suit) {

        if (rank === 'A') return 20;

        if (['10', 'J', 'Q', 'K'].includes(rank)) return 10;

        if (rank === 'Q' && suit === 'spades') return 100;

        if (['2', '3', '4', '5', '6', '7', '8', '9'].includes(rank)) return 5;

        return 0;

    }



    function shuffle(deck) {

        for (let i = deck.length - 1; i > 0; i--) {

            const j = Math.floor(Math.random() * (i + 1));

            [deck[i], deck[j]] = [deck[j], deck[i]];

        }

        return deck;

    }



    function initializeGame() {

        const deck = createDeck();

        drawPile = shuffle(deck);

        players.forEach(player => {

            player.hand = drawPile.splice(0, 13);

        });

        discardPile.push(drawPile.pop());

        updateDisplay();

    }



    function updateDisplay() {

        document.getElementById('hand-cards').innerHTML = players[0].hand.map((card, index) => `<div class='card'>${card.rank} of ${card.suit}</div>`).join('');

        document.getElementById('discard-cards').innerHTML = discardPile.map(card => `<div class='card'>${card.rank} of ${card.suit}</div>`).join('');

    }



    function drawCard() {

        const card = drawPile.pop();

        players[0].hand.push(card);

        log(`You drew ${card.rank} of ${card.suit}`);

        updateDisplay();

    }



    function log(message) {

        const logDiv = document.getElementById('log');

        logDiv.innerHTML += `<p>${message}</p>`;

        logDiv.scrollTop = logDiv.scrollHeight;

    }



    window.onload = initializeGame;

</script>



</body>

</html>
