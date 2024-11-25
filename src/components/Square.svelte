<script>
  import CircleIcon from "./Icons/CircleIcon.svelte";
  import XIcon from "./Icons/XIcon.svelte";

  const { value, onClick } = $props();
</script>

<button
  class="w-16 h-16 bg-blue-700 text-white flex items-center justify-center"
  onclick={onClick}
>
  {#if value === "X"}
    <XIcon />
  {:else if value === "O"}
    <CircleIcon />
  {/if}
</button>
