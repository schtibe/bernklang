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

<main>
  <div class="pin" bind:this={pinElement}>
    <svg
      alt="map pin by Tim Neumann from the Noun Project"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      viewBox="0 0 44 55"
      version="1.1"
      x="0px"
      y="0px"><title>Location</title>
      <desc>Created with Sketch.</desc>
      <g stroke-width="1" fill-rule="evenodd">
        <g transform="translate(-161.000000, -532.000000)">
          <path
            d="M196.723868,550.780623 C196.884802,549.981991 196.97777,549.158748 196.996478,548.317189 C196.998819,548.223689 197,548.131336 197,548.040169 C197,548.038734 196.999985,548.0373 196.999955,548.035867 C196.999985,548.023915 197,548.011959 197,548 C197,540.268014 190.731986,534 183,534 C175.268014,534 169,540.268014 169,548 C169,548.039815 169.000166,548.079591 169.000498,548.119328 C169.000166,548.126253 169,548.1332 169,548.140169 C169,556 178,573 183,573 C187.481231,573 195.17549,559.344629 196.723868,550.780623 L196.723868,550.780623 Z M183,554 C186.313708,554 189,551.313708 189,548 C189,544.686292 186.313708,542 183,542 C179.686292,542 177,544.686292 177,548 C177,551.313708 179.686292,554 183,554 L183,554 Z" />
        </g>
      </g>
    </svg>
  </div>
  <div
    bind:this={popoverElement}
    class:show={isTooltipShown}
    class:hide={!isTooltipShown}>
    {#if isTooltipShown}
      <Tooltip
        date={data.date}
        on:close={toggleTooltip}
        audioFileName={data.soundFile} />
    {/if}
  </div>
</main>
