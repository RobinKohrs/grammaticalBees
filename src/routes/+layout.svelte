<script>
  import "../app.css";
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
</svelte:head>

<slot />

<style lang="scss">
  :global(body) {
    --bgColor: bisque;

    background-color: var(--bgColor);
    font-family: "Playfair Display";
  }
</style>
