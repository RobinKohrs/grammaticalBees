<script>
  import { onMount } from "svelte";

  import { typeStore } from "../stores/currentType.js";

  // data
  import article from "./../assets/article.json";

  // components
  import TextPart from "$lib/textPart.svelte";
  import Buttons from "$lib/textparts/buttons.svelte";

  let wordTypes = [
    {
      type: "all",
      display: "Alles",
      color: "black",
    },
    {
      type: "noun",
      display: "Nomen",
      color: "red",
    },
    {
      type: "verb",
      display: "Verb",
      color: "green",
    },
    {
      type: "adj",
      display: "Adjektiv",
      color: "blue",
    },
    {
      type: "adv",
      display: "Adverb",
      color: "purple",
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
</script>

<div class="container">
  <section class="buttons">
    <div class="text-center mb-4 font-bold text-lg">Zeig mir:</div>
    {#if $typeStore}
      <Buttons {wordTypes} selected={$typeStore} on:click={setWordType} />
    {/if}
  </section>

  <section class="newspaper">
    <TextPart {wordTypes} part={"newspaper"} text={"ORF"} />
  </section>

  <section class="title">
    <TextPart part={"article-header"} text={article.header} />
  </section>

  <section class="subtitle">
    <TextPart {wordTypes} part={"article-subheader"} text={article.subheader} />
  </section>

  <section class="article-info">
    <TextPart {wordTypes} part={"article-info"} />
  </section>

  <section class="body">
    <TextPart {wordTypes} part={"body"} text={article.body} />
  </section>
</div>

<style lang="scss">
  .container {
    max-width: 600px;
    margin: 0 auto;
    padding: 0 0 1rem;
  }

  .buttons {
    position: sticky;
    top: 0;
    z-index: 999;
    background-color: hsl(0, 0, 90);
    padding-bottom: 1rem;
    border-radius: 0 0 0.5rem 0.5rem;
    border-right: 2px solid black;
    border-left: 2px solid black;
    border-bottom: 2px solid black;
  }
</style>
