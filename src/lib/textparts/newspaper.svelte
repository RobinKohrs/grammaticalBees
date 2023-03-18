<script>
  export let name = "der Standard";
</script>

<div class="title-container">
  <div class="newspaper-name">
    <span class="newspaper">{name}</span>
  </div>

  <div class="info">
    <span class="time">20.03.2023</span>
    <span class="time">7:20 Uhr</span>
    <span class="time">Wien</span>
  </div>
</div>

<style lang="scss">
  .title-container {
    & .newspaper-name {
      margin: 1rem;
      display: flex;
      justify-content: center;

      & .newspaper {
        text-align: center;
        //   white-space: nowrap;
        font-weight: 600;
        text-transform: uppercase;
        //   text-decoration: underline;
        font-size: 2.5rem;
        letter-spacing: 0.1ch;
        font-family: "Playfair Display";
      }
    }

    & .info {
      position: relative;
      display: flex;
      align-items: center;
      height: 40px;
      justify-content: space-around;
      font-family: "Droid Serif";
    }

    & .info::before {
      content: "";
      border-top: 1px solid black;
      border-bottom: 1px solid black;
      width: 100vw;
      height: 100%;
      position: absolute;
    }
  }
</style>
