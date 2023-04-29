<script>
  export let para;
  export let nParagraphsBody;

  let expanded = true;

  function handlePlus() {
    if (para >= nParagraphsBody) return;
    para = para + 1;
  }

  function handleMinus() {
    if (para <= 1) return;
    para = para - 1;
  }

  function handleShowAll() {
    para = nParagraphsBody;
  }
</script>

<div
  class="infobox-container absolute w-full bottom-0 h-36 flex flex-col justify-end"
  class:expanded
  style="background: linear-gradient(0deg, 
  hsla(256, 0%, 95%, 1) 0%, 
  hsla(256, 0%, 95%, 1) 55%, 
  hsla(256, 0%, 95%, .1) 70%); "
>
  {#if !expanded}
    <div class="controls flex justify-center gap-2 px-4 pt-6 pb-2 items-end">
      <div class="para-buttons mr-auto">
        <div>Paragraphen</div>
        <button class="button-showMore" on:click={handlePlus}>+</button>
        <button class="button-showLess" on:click={handleMinus}>-</button>
      </div>
      <div>
        <div>Info</div>
        <button
          class="open"
          on:click={() => {
            expanded = !expanded;
          }}>?</button
        >
      </div>
      <!-- <button class="button-showAll ml-auto" on:click={handleShowAll}
        >Alles</button
      > -->
    </div>
  {:else}
    <div class="info-content-container flex flex-col">
      <div class="info-content-container-inner">
        <div class="info-content-header top:">
          <br /> Grammatik-Bienen
        </div>
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
        <div class=" text-center">
          <button
            class="close"
            on:click={() => {
              expanded = !expanded;
            }}>X</button
          >
        </div>
      </div>
    </div>
  {/if}
</div>

<style lang="scss">
  .infobox-container {
    & .controls {
    }

    & button {
      font-size: 1.5rem;
      background-color: lightgray;
      padding: 0.2rem 1rem;
      border-radius: 1rem;
      border: 2px solid black;
    }
  }
  .info-content-container {
    position: fixed;
    overflow: scroll;
    inset: 0;
    z-index: 10;
    padding: 2rem;
    gap: 1rem;
    background-color: var(--infoOpacity);

    & .info-content-container-inner {
      max-width: min(90%, 600px);
      margin-inline: auto;

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
