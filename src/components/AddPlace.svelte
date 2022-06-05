<script>

    import {createEventDispatcher, getContext} from "svelte";

    const placemarkService = getContext("PlacemarkService");
    const dispatch = createEventDispatcher();

    export let categoryId;

    let placeName = "";
    let description = "";
    let latitude = "";
    let longitude = "";

    async function addPlace() {
        const place = {
            "name": placeName,
            "description": description,
            "img": [],
            "location": {
                "latitude": latitude,
                "longitude": longitude
            },
        };
        console.log(place);
        await placemarkService.addPlace(categoryId, place);
        dispatch("update");
        placeName = "";
        description = "";
        latitude = "";
        longitude = "";
    }
</script>

<form class="box" on:submit|preventDefault={addPlace}>
    <label>Enter Place Details:</label>
    <div class="field is-horizontal">
        <div class="field-body">
            <div class="field">
                <input bind:value={placeName} required class="input" type="text" placeholder="Enter Name">
            </div>
            <div class="field">
                <input bind:value={description} required class="input" type="text" placeholder="Enter description">
            </div>
            <div class="field">
                <input bind:value={latitude} required class="input" type="number" min="-90" max="90" placeholder="Enter latitude">
            </div>
            <div class="field">
                <input bind:value={longitude} required class="input" type="number" min="-180" max="180" placeholder="Enter longitude">
            </div>
        </div>
    </div>
    <button class="button is-primary">Add Place</button>
</form>