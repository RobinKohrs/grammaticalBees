<script>
  import { createEventDispatcher } from "svelte";
  import { getContext } from "svelte";

  let { body } = getContext("article");
  let totalParagraphs;
  $: if (body) {
    totalParagraphs = Object.keys(body).length - 1;
  }

  function handlePlus() {
    if (para >= totalParagraphs) return;
    para++;
  }

  function handleMinus() {
    if (para <= 1) return;
    para--;
  }

  function handleShowAll() {
    para = totalParagraphs;
  }

  export let para;
</script>

<div class="container flex justify-around flex-wrap">
  <div class="paragraph">
    <button on:click={handlePlus}>+</button>
    <span>Absatz</span>
    <button on:click={handleMinus}>-</button>
  </div>
  <div class="alles">
    <button on:click={handleShowAll}>Alles</button>
  </div>
</div>

<style lang="scss">
  .container {
    font-size: 1.5rem;

    & button {
      padding: 0.2rem;
      // border: 1px solid black;
      border-radius: 0.3rem;
      overflow: hidden;
      min-width: 50px;
    }

    & button:hover {
      background-color: hsla(0, 0%, 0, 0.1);
    }
  }
</style>
