<script>
  const Nationalities = {
    Swiss: 0,
    Argentinian: 1,
    0: "Swiss",
    1: "Argentinian",
  };

  const Leagues = {
    A: 0,
    B: 1,
    0: "A",
    1: "B",
  };

  const Teams = {
    PSG: 0,
    Manchester: 1,
    0: "PSG",
    1: "Manchester",
  };

  const Positions = {
    Forward: 0,
    Backward: 1,
    0: "Forward",
    1: "Backward",
  };

  const Attributes = [Nationalities, Leagues, Teams, Positions];

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

  const maxAttempts = 3;

  let playerToBeGuessed,
    value,
    attempts,
    autocompleteDisplayedItems,
    badGuesses,
    isGuessed;

  function newGame() {
    playerToBeGuessed = players[Math.floor(Math.random() * players.length)];
    value = "";
    autocompleteDisplayedItems = [];
    attempts = maxAttempts;
    badGuesses = [];
    isGuessed = false;
  }

  function guess(player) {
    value = "";
    if (player.id === playerToBeGuessed.id) {
      isGuessed = true;
      return;
    }
    attempts--;
    badGuesses = [...badGuesses, player];
    if (attempts === 0) {
      return;
    }
  }

  function attributeToString(attributeIndex, attribute) {
    if (attributeIndex === Attributes.length) return new Date().getFullYear() - new Date(attribute).getFullYear();
    return Attributes[attributeIndex][attribute];
  }

  $: autocompleteDisplayedItems =
    value == ""
      ? []
      : players.filter((player) =>
          player.name.toLowerCase().includes(value.toLowerCase())
        );

  $: gameFinished = attempts <= 0 || isGuessed;

  newGame();
</script>

{#if gameFinished}
  <h1>{playerToBeGuessed.name}</h1>
{/if}

<img
  src={`${playerToBeGuessed.id}.png`}
  alt="player"
  class={attempts > 0 && !isGuessed ? "filter" : ""}
/>

<button type="button" on:click={newGame}>New game</button>

{#if !gameFinished}
  <input
    type="text"
    bind:value
    placeholder={`GUESS ${maxAttempts - attempts + 1} OF ${maxAttempts}`}
  />
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
              <span class={attribute === playerToBeGuessed.attributes[i] ? 'active' : ''}>{attributeToString(i, attribute)}</span>
            </li>
          {/each}
        </ul>
      </li>
    {/each}
  </ul>
{/if}
