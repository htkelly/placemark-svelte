<script>
    import {getContext, onMount} from "svelte";
    import TitleBar from "../components/TitleBar.svelte";
    import MainNavigator from "../components/MainNavigator.svelte";
    import AddPlace from "../components/AddPlace.svelte";
    import PlaceList from "../components/PlaceList.svelte";

    export let params = {}
    let placeList = []

    const placemarkService = getContext("PlacemarkService");

    async function updatePlaces() {
        const allPlaces = await placemarkService.getAllPlaces();
        placeList = allPlaces.filter(place => place.categoryid === params.id);
    }

    onMount(async () => {
        updatePlaces();
    });

</script>

<div class="columns is-vcentered">
    <div class="column is-two-thirds">
        <TitleBar subTitle={"Check out your places"} title={"PlaceMark"}/>
    </div>
    <div class="column">
        <MainNavigator/>
    </div>
</div>

<PlaceList placeList={placeList} on:update={updatePlaces}/>
<AddPlace categoryId={params.id} on:update={updatePlaces}/>
