<script>
  import CloseButton from "$lib/closeButton.svelte";

  export let para;
  export let nParagraphsBody;
  console.log("n paragraphs body: ", nParagraphsBody);

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
  class="infobox-container relative"
  style:background-color={expanded ? "var(--infoColor)" : "transparent"}
  class:expanded
>
  {#if !expanded}
    <div class="flex justify-center gap-2 px-4">
      <button class="button-showMore" on:click={handlePlus}>+</button>
      <button class="button-showLess mr-auto" on:click={handleMinus}>-</button>
      <button
        class="open"
        on:click={() => {
          expanded = !expanded;
        }}>?</button
      >
      <button class="button-shoAll ml-auto" on:click={handleShowAll}
        >Alles</button
      >
    </div>
  {:else}
    <div class="info-content flex flex-col">
      <div class="info-content-header top:">
        test test <br /> Grammatik-Bienen
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
  {/if}
</div>

<style lang="scss">
  .infobox-container {
    & button {
      font-size: 1.5rem;
      background-color: lightgray;
      padding: 0.2rem 1rem;
      border-radius: 1rem;
      border: 2px solid black;
    }
  }
  .info-content {
    position: fixed;
    inset: 0;
    z-index: 10;
    padding: 2rem;
    gap: 1rem;
    background-color: var(--infoOpacity);

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
</style>
