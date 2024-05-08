<script>
  import { onMount } from "svelte";
  import { slide } from "svelte/transition";
  import { tick } from "svelte";
  import { browser } from "$app/environment";

  // external stuff
  import { Hamburger } from "svelte-hamburgers";
  let menuOpen;

  // own compontents
  import Modal from "$lib/Modal.svelte";
  import Controls from "../lib/Controls.svelte";
  import Wordbuttons from "$lib/textparts/buttons.svelte";

  // data
  import article from "./../assets/article.json";

  let showNParagraphs = 3;
  let showButtons = false;
  let width;
  let maxWidth = 600;

  let mainDiv;

  onMount(async () => {
    updateSpans();
  });

  import { typeStore } from "../stores/currentType.js";

  let wordTypes = [
    {
      type: "all",
      display: "Alle",
      color: "black",
    },
    {
      type: "Nomen",
      display: "Nomen",
      color: "#5F5B6B",
    },
    {
      type: "Verb",
      display: "Verben",
      color: "#DB504A",
    },
    {
      type: "Adjektiv",
      display: "Adjektive",
      color: "#00916E",
    },
    {
      type: "Adverb",
      display: "Adverben",
      color: "#C8AD55",
    },
  ];

  // function setWordType({ detail }) {
  //   let t = detail.type;

  //   // cant remove "all"
  //   if ($typeStore.length == 1 && $typeStore[0] === "all" && t === "all") {
  //     return;
  //   }

  //   //
  //   // logic
  //   //

  //   // all cant exist with something else
  //   if (t !== "all" && $typeStore[0] === "all") {
  //     $typeStore.filter((e) => e !== "all");
  //     $typeStore = [t];
  //   }

  //   if (t !== "all" && typeStore[0] !== "all") {
  //     if ($typeStore.length == 1 && t === $typeStore[0]) {
  //       return;
  //     }

  //     if ($typeStore.length == 1 && t !== $typeStore[0]) {
  //       $typeStore = [...$typeStore, t];
  //     } else if ($typeStore.length > 1) {
  //       if ($typeStore.includes(t)) {
  //         $typeStore = $typeStore.filter((e) => e !== t);
  //       } else {
  //         $typeStore = [...$typeStore, t];
  //       }
  //     }
  //   }

  //   if (t === "all") {
  //     $typeStore = ["all"];
  //   }
  // }

  // get all the spans (each word) on the current page
  let spans;
  async function updateSpans() {
    if (!browser) {
      return;
    }

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

  function findColor(ty) {
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
  async function handleClick(ele) {
    if (ele.dataset.wordType === "space") {
      return;
    } else {
      clickedWord = Object.assign({}, ele.dataset);
    }
  }
</script>

<div class="container relative" style:max-width={`${maxWidth}px`}>
  <div class="nav my-4">
    <Wordbuttons {wordTypes} {showButtons} />
  </div>

  <div class="main" bind:this={mainDiv}>
    {#if clickedWord}
      <Modal bind:clickedWord {wordTypes} />
    {/if}

    <section class="newspaper">
      <span class="newspaper" data-word-type={"Nomen"}>Ö1-Wissenschaft</span>
    </section>

    <section class="header">
      <div class="article-header">
        {#each article.header.words as word}
          {@const currentWord = word.word}
          {@const currentType = word.type}
          {@const color = findColor(currentType, currentTypes)}
          <span
            data-word-type={currentType}
            style:color
            class:hide={!currentTypes.includes(currentType) &&
              currentTypes[0] !== "all"}>{currentWord}</span
          >
        {/each}
      </div>
    </section>

    <section class="subheader">
      <div class="article-subheader flex flex-wrap justify-center">
        {#each article.subheader.words as word, i}
          {@const currentWord = word.word}
          {@const currentType = word.type}
          {@const color = findColor(currentType, currentTypes)}
          <span
            data-word-type={currentType}
            style:color
            class:hide={!currentTypes.includes(currentType) &&
              currentTypes[0] !== "all"}>{currentWord}</span
          >
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

    <section class="body px-2">
      <div class="body overflow-y-hidden text-lg mb-24">
        {#each Object.keys(article.body) as bp, i}
          {#if /para/.test(bp)}
            <p transition:slide>
              {#each article.body[bp].words as word, j}
                {@const currentWord = word.word}
                {@const currentType = word.type}
                {@const color = findColor(currentType, currentTypes)}

                <span
                  data-word-type={currentType}
                  style:color
                  class:hide={!currentTypes.includes(currentType) &&
                    currentTypes[0] !== "all"}>{currentWord}</span
                >
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
    </section>
  </div>
</div>

<style lang="scss">
  span {
    opacity: 1;
    transition-property: opacity, transform, color;
    transition-duration: 200ms, 500ms, 500ms;
    transition-timing-function: linear, linear, linear;
    white-space: pre-wrap;
  }

  span.hide {
    opacity: 0;
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
    width: min(100%, 600px);
    width: var(--fullWidth);
    margin: 0 auto;
    display: grid;
    grid-template-rows: auto 1fr auto;
    height: 100%;
  }

  .main {
    overflow-y: scroll;
    & .newspaper {
      text-align: center;
    }

    & .article-info {
      position: relative;
      display: flex;
      align-items: center;
      height: 30px;
      margin: 3rem 0 1rem;
      justify-content: space-around;
      font-family: "Droid Serif";
      border-top: 1px solid black;
      border-bottom: 1px solid black;
    }

    & .article-info::before {
      content: "Elena Deutscher";
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateY(-140%) translateX(-50%);
    }

    & .article-header {
      justify-content: center;
      display: flex;
      flex-wrap: wrap;
      font-weight: 600;
      font-size: 2rem;
      margin: 0.5rem 0;
      text-decoration: underline;
      font-family: "Playfair Display";
    }

    & .body {
      & > p {
        display: flex;
        flex-wrap: wrap;
        margin: 1rem 0;
      }

      & > h2 {
        font-size: 1.5rem;
      }
    }
  }
</style>
