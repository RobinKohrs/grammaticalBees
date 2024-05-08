<script>
  import { createEventDispatcher } from "svelte";
  import { RadioButtonGroup, RadioButton } from "carbon-components-svelte";
  const dispatch = createEventDispatcher();

  export let wordTypes;
  export let showButtons;
  import { typeStore } from "../../stores/currentType.js";

  let wordTypesFormatted;

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
  export let selWordType = "all";
  $: if (selWordType) {
    $typeStore = [selWordType];
  }
</script>

<div class="modal_wordtypes flex justify-between border-b-2 pb-4 border-black">
  {#each wordTypes as t}
    <RadioButtonGroup name="wordtypes" bind:selected={selWordType}>
      <RadioButton labelText={t.display} value={t.type} />
    </RadioButtonGroup>
  {/each}

  <!-- <div class="word-types-container flex gap-2">
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
    </div> -->
</div>

<style lang="scss">
</style>
