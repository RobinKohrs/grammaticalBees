<script>
  export let subheader = "subheader";
</script>

<div class="subheader-container">
  {#each subheader.words as word, i}
    <span>
      {word.word}
    </span>
  {/each}
</div>

<style lang="scss">
  .subheader-container {
    font-family: "Playfair Display";
  }
</style>
