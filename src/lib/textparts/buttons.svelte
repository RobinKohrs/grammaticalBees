<script>
  import { createEventDispatcher } from "svelte";
  import { slide } from "svelte/transition";
  const dispatch = createEventDispatcher();

  export let wordTypes;
  export let selected;
  export let showButtons;

  let wordTypesFormatted;
  $: if (wordTypes) {
    wordTypes = [[wordTypes[0]], wordTypes.slice(1)];
    wordTypesFormatted = true;
  }

  function handleClick(e, t) {
    dispatch("click", t);
  }

  function handleExpandClick() {
    showButtons = !showButtons;
  }
</script>

{#if wordTypesFormatted}
  <div class="container">
    <div class="buttons-container">
      <div
        class="buttons-container-text text-center mb-1 font-bold text-lg flex items-center justify-center gap-4"
        on:click={handleExpandClick}
      >
        Angezeigte Wordtypen: <div
          class="arrrow"
          class:showButtons
          style="color: transparent; text-shadow: 0 0 0 black; font-size: 1.5rem; transform: rotate({showButtons
            ? '180deg'
            : '0deg'});"
        >
          ⬇
        </div>
      </div>
      {#if showButtons}
        <div transition:slide class="buttons-container-buttons">
          {#each wordTypes as wType, i}
            <div class="first-level-sep">
              {#each wType as t}
                <button
                  style:background-color={selected[0] === "all" &&
                  t.type === "all"
                    ? "#ededed"
                    : selected.includes(t.type)
                    ? t.color
                    : ""}
                  on:click={(e) => handleClick(e, t)}>{t.display}</button
                >
              {/each}
            </div>
          {/each}
        </div>
      {/if}
    </div>
  </div>
{/if}

<style lang="scss">
  .container {
    position: sticky;
    max-width: 600px;
    height: 100px;
    top: 0;
    z-index: 9;
    padding-bottom: 1rem;

    & button {
      padding: 0.5rem;
      // border: 1px solid black;
      border-radius: 0.3rem;
      overflow: hidden;
      border: 2px solid black;
    }

    & button:hover {
      background-color: hsla(0, 0%, 0, 0.1);
    }

    & .buttons-container {
      // background-color: var(--infoOpacity);
      border-radius: 0 0 0.5rem 0.5rem;
      border-right: 2px solid black;
      border-left: 2px solid black;
      border-bottom: 2px solid black;

      .buttons-container-text {
        text-align: center;
        cursor: pointer;

        & .arrrow {
          transition: transform 0.3s ease-in-out;
        }
      }

      .buttons-container-buttons {
        display: flex;
        flex-direction: row;
        justify-content: center;
        flex-wrap: wrap;
        margin: 0.5rem 0;
        gap: 1rem;

        & > * {
          margin: 0 auto;
        }

        & > div:nth-child(2) {
          display: flex;
          gap: 0.5rem;
          flex-wrap: wrap;
          justify-content: center;
        }
      }
    }
  }
</style>
