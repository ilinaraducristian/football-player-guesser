<script>
  const Nationalities = {
    Swiss: 0,
    Argentinian: 1,
    0: 'Swiss',
    1: 'Argentinian',
  };

  const Leagues = {
    A: 0,
    B: 1,
    0: 'A',
    1: 'B'
  };

  const Teams = {
    PSG: 0,
    Manchester: 1,
    0: 'PSG',
    1: "Manchester"
  };

  const Positions = {
    Forward: 0,
    Backward: 1,
    0: 'Forward',
    1: 'Backward'
  };

  const players = [
    {
      id: 0,
      name: "Cristiano Ronaldo",
      attributes: [
        Nationalities.Argentinian,
        Leagues.A,
        Teams.Manchester,
        Positions.Backward,
        "February 5, 1985",
      ],
    },
    {
      id: 1,
      name: "Lionel Messi",
      attributes: [
        Nationalities.Swiss,
        Leagues.A,
        Teams.PSG,
        Positions.Forward,
        "June 24, 1987",
      ],
    },
  ];

  const Statuses = {
    NewGame: 0,
    Guessed: 1,
    NoAttemptsLeft: 2,
    BadGuess: 3,
  };

  const maxAttempts = 3;

  let playerToBeGuessed,
    value,
    attempts,
    autocompleteDisplayedItems,
    status, badGuesses;

  function newGame() {
    playerToBeGuessed = players[Math.floor(Math.random() * players.length)];
    value = "";
    autocompleteDisplayedItems = [];
    attempts = maxAttempts;
    status = Statuses.NewGame;
    badGuesses = [];
  }

  $: {
    if (value == "") {
      autocompleteDisplayedItems = [];
    } else {
      autocompleteDisplayedItems = players.filter((player) =>
        player.name.toLowerCase().includes(value.toLowerCase())
      );
    }
  }

  newGame();

  function guess(player) {
    if (player.id === playerToBeGuessed.id) {
      status = Statuses.Guessed;
      value = "";
      return;
    }
    attempts--;
    if (attempts === 0) {
      status = Statuses.NoAttemptsLeft;
      value = "";
      return;
    }
    status = Statuses.BadGuess;
    value = "";
    badGuesses.push(player);
  }
</script>

<img src={`${playerToBeGuessed.id}.png`} alt="player" />

<button type="button">New game</button>

{#if attempts > 0}
  <input type="text" bind:value placeholder={`GUESS ${maxAttempts-attempts} OF ${maxAttempts}`} />
{/if}

{#if autocompleteDisplayedItems.length > 0}
  <ul>
    {#each autocompleteDisplayedItems as displayedItem}
      <li>
        <button type="button" on:click={() => guess(displayedItem)}
          >{displayedItem.name}</button
        >
      </li>
    {/each}
  </ul>
{/if}

{#if badGuesses.length > 0}
  <ul>
    {#each badGuesses as player}
      <li>
        <h1>{player.name}</h1>
        <ul>
          {#each player.attributes as attribute, i}
            <li>
              <span>{attribute}</span>
              <span>{attribute === playerToBeGuessed.attributes[i]}</span>
            </li>
          {/each}
        </ul>
      </li>
    {/each}
  </ul>
{/if}