<script>
  import AutoComplete from "./AutoComplete.svelte";

  const players = [
    {
      name: "Lionel Messi",
      img: 'messi.jpeg',
      born: new Date("June 24, 1987"),
      country: "Argentina",
    },
    {
      name: "Cristiano Ronaldo",
      img: 'ronaldo.jpg',
      born: new Date("February 5, 1985"),
      country: "Portugal",
    },
    {
      name: "Mohamed Salah",
      img: 'salah.jpeg',
      born: new Date("June 15, 1992"),
      country: "Egypt",
    },
  ];

  let player, result, attempt, gameFinished;

  function guess(guessedPlayer) {
    if (guessedPlayer === player.name) {
      result = "Ai ghicit!";
      gameFinished = true;
    } else {
      attempt--;
      if (attempt === 0) {
        gameFinished = true;
        result = `Ai pierdut, nu mai ai nici o încercare!`;
      }else {
        result = `Ai greșit, mai ai ${attempt} încercări!`;
      }
    }
  }

  function newGame() {
    player = players[Math.floor(Math.random() * players.length)];
    result = "";
    attempt = 2;
    gameFinished = false;
  }

  newGame();
</script>

<button on:click={newGame}>New Game</button>
<img src={player.img} alt="">
<br />
<AutoComplete
  items={players.map((player) => player.name)}
  onSelection={guess}
  disabled={gameFinished}
/>
<br />
<h1>{result}</h1>
