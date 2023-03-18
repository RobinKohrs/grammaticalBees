<script>
  export let header;
  console.log("header: ", header);
</script>

<div class="title">
  {#each header.words as word}
    <span>{word.word}&nbsp;</span>
  {/each}
</div>

<style lang="scss">
  .title {
    justify-content: center;
    display: flex;
    flex-wrap: wrap;
    font-weight: 400;
    font-size: 2rem;
    font-family: "Playfair Display";
  }
</style>
