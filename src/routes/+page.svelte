<script>
  import { onMount } from "svelte";
  import { setContext } from "svelte";
  import { slide, fade } from "svelte/transition";
  import { tick } from "svelte";
  // import debounce from "lodash/debounce";

  // own compontents
  import ShowParagraphButtons from "$lib/showParagraphButtons.svelte";
  import Modal from "$lib/Modal.svelte";
  import Infobox from "../lib/Infobox.svelte";

  // data
  import article from "./../assets/article.json";

  let bodyContainer;
  let showNParagraphs = 1;
  let showButtons = false;
  let width;
  let showBody = false;
  $: mobile = width < 768;

  function scrollToBottom() {
    window.scrollTo({
      top: bodyContainer.scrollHeight,
      behavior: "smooth",
    });
  }

  $: if (bodyContainer) {
    let observer = new ResizeObserver((ele) => {
      scrollToBottom();
    });
    observer.observe(bodyContainer);
    updateSpans();
  }

  onMount(() => {
    updateSpans();
  });

  import { typeStore } from "../stores/currentType.js";

  setContext("article", article);

  // components
  import Buttons from "$lib/textparts/buttons.svelte";

  let wordTypes = [
    {
      type: "all",
      display: "Alles",
      color: "black",
    },
    {
      type: "Nomen",
      display: "Nomen",
      color: "#5F5B6B",
    },
    {
      type: "Verb",
      display: "Verb",
      color: "#DB504A",
    },
    {
      type: "Adjektiv",
      display: "Adjektiv",
      color: "#00916E",
    },
    {
      type: "Adverb",
      display: "Adverb",
      color: "#C8AD55",
    },
  ];

  function setWordType({ detail }) {
    let t = detail.type;

    // cant remove "all"
    if ($typeStore.length == 1 && $typeStore[0] === "all" && t === "all") {
      return;
    }

    //
    // logic
    //

    // all cant exist with something else
    if (t !== "all" && $typeStore[0] === "all") {
      $typeStore.filter((e) => e !== "all");
      $typeStore = [t];
    }

    if (t !== "all" && typeStore[0] !== "all") {
      if ($typeStore.length == 1 && t === $typeStore[0]) {
        return;
      }

      if ($typeStore.length == 1 && t !== $typeStore[0]) {
        $typeStore = [...$typeStore, t];
      } else if ($typeStore.length > 1) {
        if ($typeStore.includes(t)) {
          $typeStore = $typeStore.filter((e) => e !== t);
        } else {
          $typeStore = [...$typeStore, t];
        }
      }
    }

    if (t === "all") {
      $typeStore = ["all"];
    }
  }

  // get all the spans (each word) on the current page
  let spans;
  async function updateSpans() {
    // wait for the next tick to make sure the spans are rendered
    await tick;
    // get all the spans
    spans = document.querySelectorAll("span");
    [...spans].forEach((span) => {
      // if the span was already rendered, do not attach an event listener
      if (!span.hasAttribute("data-rendered")) {
        span.dataset.rendered = true;
        span.dataset.word = span.innerText;
        span.addEventListener("click", () => {
          handleClick(span);
        });
      }
    });
  }

  // if the number of paragraphes changes, update the spans-list
  $: if (showNParagraphs > 1) {
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

  let clickedWord;
  $: console.log("clickedWord: ", clickedWord);
  async function handleClick(ele) {
    clickedWord = Object.assign({}, ele.dataset);
  }
</script>

<!-- <svelte:window on:resize={resize} /> -->
<!-- <svelte:window bind:innerWidth={width} /> -->

{#if !showBody}
  <Infobox showWordButtons={true} bind:showBody />
{/if}
<div class="container relative" bind:this={bodyContainer}>
  {#if clickedWord}
    <Modal bind:clickedWord />
  {/if}

  {#if showBody}
    <Buttons
      {wordTypes}
      selected={$typeStore}
      on:click={setWordType}
      {showButtons}
    />

    <section class="newspaper">
      <div class="newspaper-name">
        <span class="newspaper" data-word-type={"noun"}>Ö1-Wissenschaft</span>
      </div>
    </section>

    <section class="header">
      <div class="article-header">
        {#each article.header.words as word}
          <span> {word.word}</span>
        {/each}
      </div>
    </section>

    <!-- style:width={mobile ? "100%" : "140%"}
      style:transform={mobile ? "" : "translateX(-15%)"}
      style:font-size={mobile ? "1.2rem" : "1.4rem"}
      style:margin={mobile ? "2rem 0" : "3rem 0"} -->
    <section class="subheader">
      <div class="article-subheader flex flex-wrap justify-center">
        {#each article.subheader.words as word, i}
          <span data-word-type={word.type}>{@html word.word}</span>
        {/each}
      </div>
    </section>

    <section class="info">
      <div class="article-info">
        <span class="time">20.03.2023</span>
        <span class="time">7:20 Uhr</span>
        <span class="time">Wien</span>
      </div>
    </section>

    <section class="body" bind:this={bodyContainer}>
      <div class="body overflow-y-hidden text-lg">
        {#each Object.keys(article.body).slice(0, showNParagraphs) as bp, i}
          {#if /para/.test(bp)}
            <p transition:slide>
              {#each article.body[bp].words as word, j}
                {@const nextWord =
                  j < article.body[bp].words.length
                    ? article.body[bp].words[j + 1]
                    : { type: "END" }}
                {@const currentWord = word.word}
                {@const currentType = word.type}
                {@const color = findColor(currentType, currentTypes)}

                {#if nextWord && nextWord.type === "PUNCT"}
                  <span
                    data-word-type={currentType}
                    style:color
                    class:hide={!currentTypes.includes(currentType) &&
                      currentTypes[0] !== "all"}>{currentWord}</span
                  >
                {:else}
                  <span
                    data-word-type={currentType}
                    style:color
                    class:hide={!currentTypes.includes(currentType) &&
                      currentTypes[0] !== "all"}>{currentWord}</span
                  >
                {/if}
              {/each}
            </p>
          {:else}
            <h2>
              {#each article.body[bp].words as word}
                <span>
                  {word.word}&nbsp;
                </span>
              {/each}
            </h2>
          {/if}
        {/each}
      </div>
      <ShowParagraphButtons bind:para={showNParagraphs} />
    </section>
  {/if}
</div>

<style lang="scss">
  span {
    opacity: 1;
    transition-property: opacity, transform, color;
    transition-duration: 200ms, 500ms, 500ms;
    transition-timing-function: linear, linear, linear;
    white-space: pre-wrap;
  }

  span:hover {
    background-color: hsla(0%, 0%, 0%, 0.1);
    cursor: pointer;
  }

  .modal-content {
    & > * {
      padding: 0.5rem;
      display: grid;
      align-items: center;
    }
    & .value {
      font-weight: 600;
      font-size: 1.4rem;
      min-width: 100px;
    }
  }

  .container {
    max-width: 600px;
    padding: 0 0 1rem;
    margin: 0 auto;
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
    font-weight: 600;
    font-size: 2rem;
    margin: 0.5rem 0;
    text-decoration: underline;
    font-family: "Playfair Display";
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
