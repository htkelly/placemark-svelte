<script>
    import 'leaflet/dist/leaflet.css';
    import {LeafletMap} from '../services/leaflet-map.js';
    import {get} from 'svelte/store';
    import {user} from "../stores.js"
    import {getContext, onMount} from "svelte";

    const placemarkService = getContext("PlacemarkService");

    const mapConfig = {
        location: {lat: 52.160858, lng: -7.152420},
        zoom: 8,
        minZoom: 1,
    };

    async function drawMap() {
        const storedUser = get(user);
        const allCategories = await placemarkService.getAllCategories();
        const categoryList = allCategories.filter(category => category.userid === storedUser.userId);
        const allPlaces = await placemarkService.getAllPlaces();
        const map = new LeafletMap("placemark-map", mapConfig);
        categoryList.forEach(category => {
            const categoryPlaces = allPlaces.filter(place => place.categoryid === category._id);
            map.addLayerGroup(category.name);
            categoryPlaces.forEach(place => {
                map.addMarker({lat: place.location.latitude, lng: place.location.longitude}, place.name, category.name);
            });
        });
        map.showZoomControl();
        map.showLayerControl();
    }

    onMount(async () => {
        await drawMap();
    });
</script>

<div class="box" id="placemark-map" style="height:800px">
</div>