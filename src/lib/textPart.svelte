<script>
  import { onDestroy, onMount, tick } from "svelte";
  import { slide, fade } from "svelte/transition";
  import { typeStore } from "../stores/currentType";
  import ShowParagraphButtons from "$lib/showParagraphButtons.svelte";

  let spans;
  onMount(() => {
    updateSpans();
  });

  // export props
  export let part;
  export let text;
  export let wordTypes;
  let showNParagraphs = 1;

  // get all the spans (each word) on the current page
  async function updateSpans() {
    await tick;
    spans = document.querySelectorAll("span");
    [...spans].forEach((span) => {
      // if the span was already rendered, do not attach an event listener
      if (!span.hasAttribute("rendered")) {
        span.setAttribute("rendered", true);
        span.setAttribute("flipped", false);
        span.style.tranform = "rotate3d(1, 0, 0, 360deg)";
        span.addEventListener("click", () => {
          flipWord(span);
        });
      }
    });
  }

  // if the number of paragraphes changes, update the spans-list
  $: if (showNParagraphs > 1 && part === "body") {
    updateSpans();
  }
  $: currentTypes = $typeStore;

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

  async function flipWord(ele) {
    let w = ele.getBoundingClientRect().width;
    let f = ele.getAttribute("flipped");
    console.log("f: ", f);

    if (f === "false") {
      console.log("here");
      ele.style.transform = "rotate3d(1, 0, 0, 360deg)";
      ele.style.width = `${w}px`;
      ele.style.color = "red";
      ele.style.textAlign = "center";
      ele.setAttribute("flipped", true);
      ele.innerText = "flipped";
    } else {
      ele.style.transform = "rotate3d(0, 0, 0, 0deg)";
      ele.setAttribute("flipped", false);
    }
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
  <div class="body overflow-auto">
    {#each Object.keys(text).slice(0, showNParagraphs) as bp, i}
      {#if /para/.test(bp)}
        <p transition:slide>
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

  <ShowParagraphButtons bind:para={showNParagraphs} />
{/if}

<style lang="scss">
  span {
    opacity: 1;
    transition-property: opacity, transform;
    transition-duration: 200ms, 500ms;
    transition-timing-function: linear, linear;
  }

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
