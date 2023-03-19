<script>
  import { onMount } from "svelte";
  import { typeStore } from "../stores/currentType";
  import ShowParagraphButtons from "$lib/showParagraphButtons.svelte";

  $: currentTypes = $typeStore;
  // export props
  export let part;
  export let text;
  export let wordTypes;

  let showNParagraphs = 1;

  function findColor(ty, types) {
    let color = "black";
    let found = wordTypes.find((t) => t.type == ty);
    if (found) {
      color = found["color"];
    }

    if (currentTypes[0] === "all") {
      color = "black";
    }

    return color;
  }
</script>

{#if part === "newspaper"}
  <div class="newspaper-name">
    <span class="newspaper">{text}</span>
  </div>
{/if}

{#if part === "article-header"}
  <div class="article-header">
    {#each text.words as word}
      <span> {word.word}&nbsp;</span>
    {/each}
  </div>
{/if}

{#if part === "article-subheader"}
  <div class="article-subheader">
    {#each text.words as word, i}
      <span>
        {word.word}
      </span>
    {/each}
  </div>
{/if}

{#if part === "article-info"}
  <div class="article-info">
    <span class="time">20.03.2023</span>
    <span class="time">7:20 Uhr</span>
    <span class="time">Wien</span>
  </div>
{/if}

{#if part === "body"}
  <div class="body">
    {#each Object.keys(text).slice(0, showNParagraphs) as bp, i}
      {#if /para/.test(bp)}
        <p>
          {#each text[bp].words as word, j}
            {@const nextWord =
              j < text[bp].words.length
                ? text[bp].words[j + 1]
                : { type: "END" }}
            {@const currentWord = word.word}
            {@const currentType = word.type}
            {@const color = findColor(currentType, currentTypes)}

            {#if nextWord && nextWord.type === "PUNCT"}
              <span
                style:color
                class:hide={!currentTypes.includes(currentType) &&
                  currentTypes[0] !== "all"}
              >
                {currentWord}
              </span>
            {:else}
              <span
                style:color
                class:hide={!currentTypes.includes(currentType) &&
                  currentTypes[0] !== "all"}
              >
                {currentWord}&nbsp;
              </span>
            {/if}
          {/each}
        </p>
      {:else}
        <h2>
          {#each text[bp].words as word}
            <span>
              {word.word}&nbsp;
            </span>
          {/each}
        </h2>
      {/if}
    {/each}
  </div>

  <ShowParagraphButtons />
{/if}

<style lang="scss">
  .newspaper-name {
    margin: 1rem;
    display: flex;
    justify-content: center;

    & .newspaper {
      text-align: center;
      //   white-space: nowrap;
      font-weight: 600;
      text-transform: uppercase;
      //   text-decoration: underline;
      font-size: 1.2rem;
      letter-spacing: 0.1ch;
      font-family: "Playfair Display";
    }
  }

  .article-info {
    position: relative;
    display: flex;
    align-items: center;
    height: 30px;
    margin: 4rem 2rem 0 0;
    justify-content: space-around;
    font-family: "Droid Serif";
    border-top: 1px solid black;
    border-bottom: 1px solid black;
  }

  .article-info::before {
    content: "Elena Deutscher";
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateY(-140%) translateX(-50%);
  }

  .article-header {
    justify-content: center;
    display: flex;
    flex-wrap: wrap;
    font-weight: 400;
    font-size: 2rem;
    margin: 0.5rem 0;
    text-decoration: underline;
    font-family: "Playfair Display";
  }

  .article-subheader {
    font-family: "Playfair Display";
    line-height: 1.8rem;
    font-weight: 700;
  }

  .body {
    & span {
      opacity: 1;
      transition: opacity 200ms linear;
    }

    & > p {
      display: flex;
      flex-wrap: wrap;
      margin: 1rem 0;
    }

    & > h2 {
      font-size: 1.5rem;
    }

    & .hide {
      opacity: 0;
    }
  }
</style>
