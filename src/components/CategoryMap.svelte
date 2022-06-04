<script>
    import 'leaflet/dist/leaflet.css';
    import {LeafletMap} from '../services/leaflet-map.js';
    import {getContext, onMount} from "svelte";

    const placemarkService = getContext("PlacemarkService");

    export let categoryId, categoryName;

    const mapName = categoryName + "_map"

    const mapConfig = {
        location: {lat: 52.160858, lng: -7.152420},
        zoom: 8,
        minZoom: 1,
    };

    async function drawCategoryMap() {
        const allPlaces = await placemarkService.getAllPlaces();
        const categoryPlaces = allPlaces.filter(place => place.categoryid === categoryId);
        const map = new LeafletMap(mapName, mapConfig);
        map.addLayerGroup(categoryName);
        categoryPlaces.forEach(place => {
            map.addMarker({lat: place.location.latitude, lng: place.location.longitude}, place.name, categoryName);
        });
        map.showZoomControl();
        map.showLayerControl();
    }

    onMount(async () => {
        await drawCategoryMap();
    });
</script>

<div class="box" id={mapName} style="height:800px">
</div>