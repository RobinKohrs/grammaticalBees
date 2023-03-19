<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let wordTypes;
  export let selected;
  $: console.log("selected: ", selected);

  function handleClick(e, type) {
    dispatch("click", type);
  }
</script>

<div class="buttonContainer">
  {#each wordTypes as type, i}
    {console.log("type: ", type) || ""}
    <button
      style="margin-right: {i === 0 ? 'auto' : ''};"
      style:background-color={selected[0] === "all" && type.type === "all"
        ? "grey"
        : selected.includes(type.type)
        ? type.color
        : ""}
      on:click={(e) => handleClick(e, type)}>{type.display}</button
    >
  {/each}
</div>

<style lang="scss">
  .buttonContainer {
    margin: 0 2rem 0;
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    flex-wrap: wrap;

    & > button {
      padding: 0.2rem;
      // border: 1px solid black;
      border-radius: 0.3rem;
      overflow: hidden;
    }

    & > button:hover {
      background-color: hsla(0, 0%, 0, 0.1);
    }
  }
</style>
