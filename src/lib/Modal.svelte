<script>
  import CloseButton from "$lib/closeButton.svelte";

  export let clickedWord;
  function handleClick() {
    console.log("handleClick");
    clickedWord = undefined;
  }
</script>

<div class="modal-container fixed inset-0 grid place-items-center z-10">
  <div
    class="modal-content-container relative border-2 border-slate-500 bg-slate-100 rounded-lg"
  >
    <CloseButton top={0} right={0} {handleClick} />
    <!-- <div class="modal-close absolute top-0 right-0">
      <button
        class="bg-zinc-700 text-slate-100 rounded-full"
        style="height: 30px; aspect-ratio: 1; border-radius: 50%; transform: translateX(50%) translateY(-50%);"
        on:click={() => (clickedWord = undefined)}
      >
        X
      </button>
    </div> -->
    <div class="modal-content flex flex-col items-stretch">
      <div class="word rounded-t-lg">
        <div class="key">Wort:</div>
        <div class="value">{clickedWord.word}</div>
      </div>
      <div class="type">
        <div class="key ">Worttyp:</div>
        <div class="value">{clickedWord.wordType}</div>
      </div>
      <div class="value rounded-b-lg">
        <a
          href={`https://www.duden.de/suchen/dudenonline/${clickedWord.word}`}
          target="_blank"
          >Duden-Link
        </a><span
          class="px-2"
          style="color: transparent; font-size: .8rem; text-shadow: 0 0 0 black;"
          >🔗</span
        >
      </div>
    </div>
  </div>
</div>

<style lang="scss">
  .modal-container {
    background-color: var(--infoOpacity);
    & .modal-content-container {
      width: 80%;
      max-width: 400px;

      & .modal-content {
        & > * {
          padding: 0.5rem;
          display: flex;
          flex-wrap: wrap;
          justify-content: space-between;
          align-items: center;
        }

        & > :nth-child(odd) {
          background-color: hsla(0, 0%, 0, 0.1);
        }

        & :nth-child(3) {
          justify-content: center;
          & a {
            text-decoration: underline;
          }
        }

        & .value {
          font-weight: 600;
          font-size: 1.4rem;
          min-width: 100px;
        }
      }
    }
  }
</style>
