<script context="module">
  let closeLastOpen = () => {};
</script>

<script>
  export let data;

  import Overlay from "ol/Overlay";
  import Tooltip from "./Tooltip.svelte";
  import { fromLonLat } from "ol/proj";
  import { afterUpdate, onMount, getContext } from "svelte";

  import { createPopper } from "@popperjs/core";

  const pos = fromLonLat(data.coords);

  const { getMap } = getContext("map");

  let map;

  let interval = null;

  let pinElement;
  let popoverElement;
  let isTooltipShown = false;

  const addMarker = () => {
    let marker = new Overlay({
      position: pos,
      positioning: "center-center",
      element: pinElement,
      stopEvent: false,
    });

    map.addOverlay(marker);
    marker.element.addEventListener("click", toggleTooltip);
  };

  const addPopup = () => {
    const popup = new Overlay({
      element: popoverElement,
      position: pos,
    });

    createPopper(pinElement, popoverElement, {
      placement: "top",
      animation: true,
    });

    map.addOverlay(popup);
  };

  function toggleTooltip() {
    isTooltipShown = !isTooltipShown;
    if (isTooltipShown) {
      closeLastOpen();
    }

    closeLastOpen = () => {
      isTooltipShown = false;
    };
  }

  afterUpdate(() => {
    interval = setInterval(() => {
      map = getMap();
      if (map) {
        clearInterval(interval);
        addMarker();
        addPopup();
      }
    }, 10);
  });

  onMount(() => {});
</script>

<div class="pin" bind:this={pinElement}>
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 44 55"
    ><path
      d="M35.724 18.78A14.043 14.043 0 0036 16c0-7.732-6.268-14-14-14S8 8.268 8 16a14.283 14.283 0 000 .14C8 24 17 41 22 41c4.481 0 12.175-13.655 13.724-22.22zM22 22a6 6 0 100-12 6 6 0 000 12z"
      fill-rule="evenodd"
    /></svg
  >
</div>
<div
  bind:this={popoverElement}
  class:show={isTooltipShown}
  class:hide={!isTooltipShown}
>
  {#if isTooltipShown}
    <Tooltip
      date={data.date}
      on:close={toggleTooltip}
      audioFileName={data.soundFile}
    />
  {/if}
</div>

<style>
  .pin {
    height: 32px;
    width: 32px;
    color: rgba(212, 39, 9, 0.801);
    cursor: pointer;
  }

  svg {
    fill: currentColor;
  }

  .hide {
    display: none;
  }

  .show {
    display: block;
  }
</style>
