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
      display: "Alle",
    },
    {
      type: "noun",
      display: "Nomen",
    },
    {
      type: "verb",
      display: "Verb",
    },
    {
      type: "adj",
      display: "Adjektiv",
    },
    {
      type: "adv",
      display: "Adverb",
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
  <section class="newspaper">
    <TextPart part={"newspaper"} text={"ORF"} />
  </section>

  <section class="title">
    <TextPart part={"article-header"} text={article.header} />
  </section>

  <section class="subtitle">
    <TextPart part={"article-subheader"} text={article.subheader} />
  </section>

  <section class="article-info">
    <TextPart part={"article-info"} />
  </section>

  <section class="body">
    <TextPart part={"body"} text={article.body} />
  </section>
  {#if $typeStore}
    <Buttons {wordTypes} selected={$typeStore} on:click={setWordType} />
  {/if}
</div>

<style lang="scss">
  .container {
    max-width: 600px;
    margin: 0 auto;
    padding: 1rem;
  }
</style>
