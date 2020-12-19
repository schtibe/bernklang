<script>
	import Pin from "./Pin.svelte";
	import { onMount } from "svelte";

	import Map from "ol/Map";
	import View from "ol/View";
	import TileLayer from "ol/layer/Tile";
	import OSM from "ol/source/OSM";
	import { fromLonLat } from "ol/proj";

	let map;

	let pins = [
		{
			coords: [7.39416667, 46.94138889],
		},
		{
			coords: [7.39888889, 46.94138889],
		},
	];

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
			}),
		});
	};

	const addPins = () => {};

	onMount(() => {
		createMap();
		addPins();
	});
</script>

<style>
	body {
		margin: 0;
		padding: 0;
	}

	#map {
		width: 100vw;
		height: 100vh;
	}
</style>

<main>
	<div id="map" />
	<div style="display: none;">
		{#each pins as data}
			<Pin {map} {data} />
		{/each}
	</div>
</main>
