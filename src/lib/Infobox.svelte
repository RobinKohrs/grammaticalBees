<script>
  import CloseButton from "$lib/closeButton.svelte";

  export let showWordButtons;
  $: console.log("expanded", showWordButtons);

  export let expanded = true;
  export let showBody = false;
</script>

<div
  class="infobox-container"
  style:background-color={expanded ? "var(--infoColor)" : "transparent"}
  class:expanded
>
  {#if !expanded}
    <CloseButton
      position={"fixed"}
      top={showWordButtons ? "calc(100vh - 60px)" : "10px"}
      left={"10%"}
      transform={false}
      size={50}
      icon={"?"}
      handleClick={() => {
        showBody = false;
        expanded = !expanded;
      }}
    />
  {:else}
    <CloseButton
      position={"fixed"}
      top={showWordButtons ? "calc(100vh - 60px)" : "10px"}
      left={"10%"}
      size={50}
      transform={false}
      handleClick={() => {
        showBody = true;
        expanded = !expanded;
      }}
    />
    <div class="info-content relative flex flex-col">
      <div class="info-content-header top:">Grammatik-Bienen</div>
      <div class="info-content-content">
        <h2>Was kann man hier machen?</h2>
        <p>
          In dem Text können die einzelnen Wörter <strong>angeklickt</strong>
          werden. Durch das Klicken erscheint ein kleines Fenster mit Informationen
          zu dem Wort. Wie z.B. der <strong>Worttyp</strong> oder ein Link zu der
          Definition.
        </p>
        <p>
          Auch können einzelne Worttypen, wie Nomen, Verben oder Adjektive
          angezeigt werden. Dazu klickt man auf die entsprechenden <strong
            >Knöpfe ganz oben auf der Seite.</strong
          >
        </p>
        <h2>Woher kommt der Text?</h2>
        <p>
          Der Text kommt von <strong>Science ORF</strong>. Zu erreichen unter
          <a href="https://science.orf.at/stories/3215754/">diesem Link</a>.
          Erschienen ist der Text am
          <strong>27. Oktober 2022</strong> und ist von
          <strong>Elena Deutscher</strong>
        </p>
      </div>
    </div>
  {/if}
</div>

<style lang="scss">
  .infobox-container {
    position: absolute;
    z-index: 200;
    left: 0;
    top: 0;
    transition: all 0.2s linear;
    width: 100vw;
    min-height: 100vh;
    display: grid;
    justify-items: center;

    // &.expanded {
    //   width: 100vw;
    //   height: 100vh;
    // }

    .info-content {
      padding: 2rem;
      max-width: 600px;

      & .info-content-header {
        font-size: 4rem;
        font-size: clamp(2rem, 4vw, 5rem);
        font-weight: 700;
        margin-bottom: 1rem;
        text-align: center;
        text-decoration: underline;
      }

      h2 {
        font-size: 1.5rem;
        margin: 1rem 0;
        text-align: center;
        font-weight: 700;
        letter-spacing: 0.1;
      }

      a {
        color: var(--textColor);
        text-decoration: underline;
      }
    }
  }
</style>
