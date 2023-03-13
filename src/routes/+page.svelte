<script>
  import text from "./../assets/text.json";
</script>

<h1>The text</h1>
<div class="container">
  <span>
    {JSON.stringify(text)}
  </span>
</div>

<style lang="scss">
  .container {
    max-width: 400px;
    margin: 0 auto;
    & pre {
      color: darkblue;
    }
  }
</style>
