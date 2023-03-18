<script>
  import { onMount } from "svelte";
  import { typeStore } from "../stores/currentType";

  $: currentType = $typeStore;

  // export props
  export let part;
  export let text;
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
    {#each Object.keys(text) as bp, i}
      {#if /para/.test(bp)}
        <p>
          {#each text[bp].words as word, j}
            {@const nextWord =
              j < text[bp].words.length
                ? text[bp].words[j + 1]
                : { type: "END" }}
            {#if nextWord && nextWord.type === "PUNCT"}
              <span>
                {word.word}
              </span>
            {:else}
              <span class="show">
                {word.word}&nbsp;
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
      font-size: 2.5rem;
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
    content: "Robin Kohrs";
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
    font-family: "Playfair Display";
  }

  .article-subheader {
    font-family: "Playfair Display";
    line-height: 1.8rem;
    font-weight: 700;
  }

  .body {
    & > p {
      display: flex;
      flex-wrap: wrap;
      margin: 1rem 0;
    }

    & > h2 {
      font-size: 1.5rem;
    }
  }
</style>
