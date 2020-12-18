<script>
	import { onMount } from "svelte";

	import Map from "ol/Map";
	import View from "ol/View";
	import TileLayer from "ol/layer/Tile";
	import OSM from "ol/source/OSM";
	import Vector from "ol/layer/Vector";
	import Point from "ol/geom/Point";
	import VectorSource from "ol/source/Vector";
	import Feature from "ol/Feature";

	import { fromLonLat } from "ol/proj";

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
			}),
		});
	};

	const addVectorLayer = () => {
		var layer = new Vector({
			source: new VectorSource({
				features: [
					new Feature({
						geometry: new Point(fromLonLat([7.4331, 46.9428])),
					}),
				],
			}),
		});

		map.addLayer(layer);
	};

	onMount(() => {
		createMap();
		addVectorLayer();
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
</main>
