<script>
  import "ol/ol.css";
  import Pin from "./Pin.svelte";
  import Header from "./Header.svelte";
  import { onMount, setContext } from "svelte";

  import Map from "ol/Map";
  import View from "ol/View";
  import TileLayer from "ol/layer/Tile";
  import OSM from "ol/source/OSM";
  import { fromLonLat } from "ol/proj";

  import pinData from "./data.json";

  let map;

  const createMap = () => {
    map = new Map({
      target: "map",
      layers: [
        new TileLayer({
          source: new OSM(),
        }),
      ],
      view: new View({
        center: fromLonLat([7.4331, 46.9428]),
        zoom: 14,
        minZoom: 14,
        maxZoom: 18,
      }),
    });
  };

  const addPins = () => {};

  onMount(() => {
    createMap();
    addPins();
  });

  setContext("map", {
    getMap: () => map,
  });
</script>

<main>
  <div id="map" />
  <Header />
  <div>
    {#each pinData as data}
      <Pin {data} />
    {/each}
  </div>
</main>

<style>
  #map {
    width: 100vw;
    height: 100vh;
  }
</style>
