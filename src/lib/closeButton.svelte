<script>
  export let top;
  export let left;
  export let right;
  export let bottom;
  export let position = "absolute";
  export let transform = true;
  export let size = 30;

  export let icon = "X";

  export let handleClick;
</script>

<div
  class="modal-close {position}"
  style:top
  style:left
  style:bottom
  style:right
>
  <button
    class="bg-zinc-700 text-slate-100 rounded-full"
    style="height: {size}px; aspect-ratio: 1; border-radius: 50%; transform: {transform
      ? 'translateX(50%) translateY(-50%)'
      : 'none'};"
    on:click={handleClick}
  >
    {@html icon}
  </button>
</div>

<style lang="scss">
  div {
    transition: all 0.2s linear;
  }
</style>
