<script>
  export let date;
  export let audioFileNames;

  import Close from "./Close.svelte";
  import { createEventDispatcher } from "svelte";
  import AudioViz from "./AudioViz.svelte";

  const audioFiles = audioFileNames.map((name) => `assets/sounds/${name}`);

  const dispatch = createEventDispatcher();

  function close() {
    dispatch("close");
  }
</script>

<div class="tooltip">
  <div class="close">
    <Close on:close={close} />
  </div>

  <div class="date">{date}</div>
  <div class="audio-viz">
    {#each audioFiles as audioFile}
      <AudioViz {audioFile} />
    {/each}
  </div>
</div>

<style>
  .tooltip {
    z-index: 1000;
    background: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.2);
    margin-bottom: 0.5em;
    display: flex;
    flex-direction: column;
    min-width: 300px;
  }

  .date {
    text-align: center;
    flex-grow: 1;
    font-size: 1.5rem;
  }

  .audio-viz {
    margin-top: 2rem;
  }

  .close {
    position: absolute;
    right: 10px;
    top: 10px;
  }
</style>
