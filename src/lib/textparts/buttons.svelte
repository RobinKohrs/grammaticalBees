<script>
  import { createEventDispatcher } from "svelte";
  import { slide } from "svelte/transition";
  const dispatch = createEventDispatcher();

  export let wordTypes;
  export let selected;
  export let showButtons;

  let wordTypesFormatted;
  $: if (wordTypes) {
    wordTypes = [[wordTypes[0]], wordTypes.slice(1)];
    wordTypesFormatted = true;
  }

  function handleClick(e, t) {
    if (selected.includes(t.type)) {
      e.target.style.background = "transparent";
    } else {
      e.target.style.background = t.color;
    }

    dispatch("click", t);
  }

  function handleMouseOver(e, t) {
    if (selected.includes(t.type)) {
      return;
    }

    e.target.style.background = t.color;
  }

  function handleMouseOut(e, type) {
    if (selected.includes(type)) {
      return;
    }

    e.target.style.background = "transparent";
  }
</script>

<div class="modal_wordtypes">
  {#if wordTypesFormatted}
    <div class="text-center font-bold text-2xl my-4">
      Wähle die angezeigten Wortarten
    </div>
    <div class="word-types-container flex gap-2">
      {#each wordTypes as wType, i}
        {#each wType as t}
          {@const hover = `bg-[${t.color}]`}
          <button
            on:mouseover={(e) => handleMouseOver(e, t)}
            on:mouseleave={(e) => handleMouseOut(e, t.type)}
            on:click={(e) => handleClick(e, t)}
            class="px-2 py-1 rounded-sm"
            style:border={`2px solid ${t.color}`}
            style:margin-right={t.type === "all" ? "auto" : ""}
          >
            {t.display}
          </button>
        {/each}
      {/each}
    </div>
  {/if}
</div>

<style lang="scss">
</style>
