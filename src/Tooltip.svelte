<script>
  export let date;
  export let audioFileName;

  import Close from "./Close.svelte";
  import { createEventDispatcher } from "svelte";
  import AudioViz from "./AudioViz.svelte";

  const audioFile = `assets/sounds/${audioFileName}`;

  const dispatch = createEventDispatcher();

  function close() {
    dispatch("close");
  }
</script>

<div class="tooltip">
  <div class="flex" on:click={close}>
    <div class="date">{date}</div>

    <div class="close">
      <Close />
    </div>
  </div>
  <div class="mt-4">
    <AudioViz {audioFile} />
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

  .flex {
    display: flex;
  }

  .justify-between {
    justify-content: space-between;
  }

  .mt-4 {
    margin-top: 1rem;
  }

  .close {
    margin-left: auto;
  }
</style>
