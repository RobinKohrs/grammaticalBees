<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let wordTypes;
  export let selected;

  let wordTypesFormatted;
  $: if (wordTypes) {
    wordTypes = [[wordTypes[0]], wordTypes.slice(2)];
    wordTypesFormatted = true;
  }

  function handleClick(e, t) {
    dispatch("click", t);
  }
</script>

{#if wordTypesFormatted}
  <div class="buttonContainer">
    {#each wordTypes as wType, i}
      <div>
        {#each wType as t}
          <button
            style:background-color={selected[0] === "all" && t.type === "all"
              ? "#ededed"
              : selected.includes(t.type)
              ? t.color
              : ""}
            on:click={(e) => handleClick(e, t)}>{t.display}</button
          >
        {/each}
      </div>
    {/each}
  </div>
{/if}

<style lang="scss">
  .buttonContainer {
    margin: 0 2rem 0;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;

    & > div:nth-child(2) {
      display: flex;
      gap: 1rem;
    }

    & button {
      padding: 0.5rem;
      // border: 1px solid black;
      border-radius: 0.3rem;
      overflow: hidden;
      border: 2px solid black;
    }

    & button:hover {
      background-color: hsla(0, 0%, 0, 0.1);
    }
  }
</style>
