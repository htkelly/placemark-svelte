<script>
    import TitleBar from "../components/TitleBar.svelte";
    import MainNavigator from "../components/MainNavigator.svelte";
    import ImageList from "../components/ImageList.svelte";
    import {getContext, onMount} from "svelte";
    import AddImage from "../components/AddImage.svelte";

    export let params = {}
    let placeName;
    let categoryId;
    let images = [];

    const placemarkService = getContext("PlacemarkService");

    async function updateGallery() {
        let allPlaces = await placemarkService.getAllPlaces();
        let thisPlace = allPlaces.filter(place => place._id === params.id)[0];
        placeName = thisPlace.name;
        categoryId = thisPlace.categoryid;
        images = thisPlace.img;
    }

    onMount(async () => {
        updateGallery();
    });

</script>

<div class="columns is-vcentered">
    <div class="column is-two-thirds">
        <TitleBar subTitle={"Check out your images"} title={"PlaceMark"}/>
    </div>
    <div class="column">
        <MainNavigator/>
    </div>
</div>

<h1 class="title">Images from {placeName}</h1>
<a href="/#/category/{categoryId}">
    <i class="fas fa-arrow-circle-left fa-2x" style="color:rgb(95, 96, 173)"></i>
</a>
<div class="section">
    <ImageList placeId={params.id} images={images} on:update={updateGallery}/>
</div>
<div class="section">
    <AddImage placeId={params.id} on:update={updateGallery}/>
</div>
