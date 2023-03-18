<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let wordTypes;
  export let selected;

  function handleClick(e, type) {
    dispatch("click", type);
  }
</script>

<div class="buttonContainer">
  {#each wordTypes as type, i}
    <button
      class:selected={selected.includes(type.type)}
      on:click={(e) => handleClick(e, type)}>{type.display}</button
    >
  {/each}
</div>

<style lang="scss">
  .buttonContainer {
    margin: 2rem 0;
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    flex-wrap: wrap;

    & > button {
      padding: 1rem;
      // border: 1px solid black;
      border-radius: 0.3rem;
      overflow: hidden;
    }

    & > button:hover {
      background-color: hsla(0, 0%, 0, 0.1);
    }

    & > button.selected {
      background-color: hsla(0, 10%, 0, 0.3);
    }
  }
</style>
