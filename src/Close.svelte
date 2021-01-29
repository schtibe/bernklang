<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  function close() {
    dispatch("close");
  }
</script>

<button class="close" on:click={close}>
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
    ><path
      d="M9.172 16.242L12 13.414 14.828 16.242 16.242 14.828 13.414 12 16.242 9.172 14.828 7.758 12 10.586 9.172 7.758 7.758 9.172 10.586 12 7.758 14.828z"
    />
    <path
      d="M12,22c5.514,0,10-4.486,10-10S17.514,2,12,2S2,6.486,2,12S6.486,22,12,22z M12,4c4.411,0,8,3.589,8,8s-3.589,8-8,8 s-8-3.589-8-8S7.589,4,12,4z"
    />
  </svg>
</button>

<style>
  button {
    background: none;
    border: none;
    color: rgb(241, 118, 17);
  }

  svg {
    fill: currentColor;
    width: 36px;
    height: 36px;
    cursor: pointer;
  }

  button:hover {
    color: rgb(255, 163, 87);
  }
</style>
