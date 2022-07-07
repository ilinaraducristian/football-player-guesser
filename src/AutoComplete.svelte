<script>
  export let onSelection = (_) => {};
  export let items = [];
  export let disabled = false;
  let displayedItems = [];
  let value = "";

  $: {
    if (value === "") {
      displayedItems = [];
    } else {
      displayedItems = items.filter((item) =>
        item.toLowerCase().includes(value.toLowerCase())
      );
    }
  }

  function select(item) {
    value = "";
    onSelection(item);
  }
</script>

<input type="text" bind:value disabled={disabled}/>

{#if displayedItems.length > 0}
  <ul>
    {#each displayedItems as item}
      <li>
        <button type="button" on:click={() => select(item)}>{item}</button>
      </li>
    {/each}
  </ul>
{/if}
