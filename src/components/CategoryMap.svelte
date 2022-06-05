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
        const firstPlace = categoryPlaces[0];
        if (firstPlace){
            mapConfig.location.lat = firstPlace.location.latitude;
            mapConfig.location.lng = firstPlace.location.longitude;
        }
        const map = new LeafletMap(mapName, mapConfig);
        map.addLayerGroup(categoryName);
        categoryPlaces.forEach(place => {
            const popUpText = `It's ${place.temperature} °C at ${place.name} and the weather is ${place.weatherDescription}`
            map.addMarker({lat: place.location.latitude, lng: place.location.longitude}, popUpText, categoryName);
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