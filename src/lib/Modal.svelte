<script>
  import { fade } from "svelte/transition";
  import { clickOutside } from "$lib/clickOutside";

  export let clickedWord;
  export let wordTypes;
  function handleClick() {
    clickedWord = undefined;
  }

  // show div only for 2 seconds
  let showWord;
  let typeColor;
  $: console.log("col: ", typeColor);
  $: if (clickedWord) {
    showWord = true;
    let wt = wordTypes.find((t) => t.type === clickedWord.wordType);
    if (wt !== undefined) {
      typeColor = wt.color;
    }
  }

  function handleClickOutside() {
    showWord = false;
  }
</script>

{#if showWord}
  <div
    class="fixed left-0 w-screen h-screen background-[rgba(0,0,0,0.4)] backdrop-blur-[3px] z-[20]"
  >
    <div
      use:clickOutside
      on:click_outside={handleClickOutside}
      class="flex flex-col justify-center p-4 absolute min-h-[140px] rounded-sm top-1/2 -translate-y-1/2 right-0 left-0 max-w-[600px] mx-auto"
      style="background-color: rgba(255,255,255, .77);"
    >
      <div class="text-2xl mb-auto text-center underline">
        {clickedWord.word}
      </div>
      <div class="text-4xl mb-auto text-center" style="color: {typeColor}">
        {clickedWord.wordType}
      </div>
    </div>
  </div>
{/if}
