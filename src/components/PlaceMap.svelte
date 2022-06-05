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
        const firstCategory = categoryList[0];
        const firstPlace = allPlaces.filter(place => place.categoryid === firstCategory._id)[0];
        if (firstPlace) {
            mapConfig.location.lat = firstPlace.location.latitude;
            mapConfig.location.lng = firstPlace.location.longitude;
        }
        const map = new LeafletMap("placemark-map", mapConfig);
        categoryList.forEach(category => {
            const categoryPlaces = allPlaces.filter(place => place.categoryid === category._id);
            map.addLayerGroup(category.name);
            categoryPlaces.forEach(place => {
                const popUpText = `It's ${place.temperature} °C at ${place.name} and the weather is ${place.weatherDescription}`
                map.addMarker({lat: place.location.latitude, lng: place.location.longitude}, popUpText, category.name);
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