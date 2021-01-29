<script>
  export let isPlaying;
  export let isLoaded;

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  function togglePlay() {
    dispatch("togglePlay");
  }
</script>

<button
  on:click={togglePlay}
  class:playing={isPlaying}
  class:disabled={!isLoaded}
>
  {#if isPlaying}
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="210mm"
      height="210mm"
      viewBox="0 0 210 210"
      ><path
        d="M24.126 23.181h61.72v163.862h-61.72zM124.155 22.957h61.72V186.82h-61.72z"
      /></svg
    >
  {:else}
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="210mm"
      height="210mm"
      viewBox="0 0 210 210"
      ><path
        d="M31.24 190.17V19.83L105 62.415 178.76 105 105 147.585z"
        fill-rule="evenodd"
      /></svg
    >
  {/if}
</button>

<style>
  button {
    background: rgb(255, 193, 59);
    border: none;
    display: flex;
    align-items: center;
    justify-content: space-around;
    height: 70px;
    width: 70px;
  }

  button.playing {
    background: rgb(241, 118, 17);
  }

  button:hover {
    background: rgb(255, 163, 87);
  }

  button.disabled {
    background: grey;
  }

  button svg {
    height: 36px;
    width: 36px;
    fill: currentColor;
    color: white;
  }
</style>
