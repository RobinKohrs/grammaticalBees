# TODOS:

- in R already set in each span a class like "header", "subheader" so that you can render all with one each call and just set
  the appropriate classes

```svelte
{#each spans as span}
<span
class={span.class} // e.g. "header"
>{span.text}</span>
{/each}
```

- [ ] close the modal
- [ ] make expand for button selection
- [ ] make introduction modal
