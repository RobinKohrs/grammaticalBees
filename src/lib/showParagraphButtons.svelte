<script>
</script>

<div class="container flex justify-around flex-wrap">
  <div class="paragraph">
    <button>+</button>
    <span>Absatz</span>
    <button>-</button>
  </div>
  <div class="alles">
    <button>Alles</button>
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
