<script>
  export let date;
  export let audioFileName;

  import Close from "./Close.svelte";
  import { createEventDispatcher } from "svelte";

  const audioFile = `assets/sounds/${audioFileName}`;

  const dispatch = createEventDispatcher();

  function close() {
    dispatch("close");
  }
</script>

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
</style>

<main>
  <div class="tooltip">
    <div class="flex justify-between" on:click={close}>
      <div>{date}</div>

      <Close />
    </div>
    <audio class="mt-4" controls src={audioFile}>
      Your browser does not support the
      <code>audio</code>
      element.
      <track kind="captions" />
    </audio>
  </div>
</main>
