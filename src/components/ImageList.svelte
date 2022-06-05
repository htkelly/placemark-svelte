<script>
    import {createEventDispatcher, getContext} from "svelte";
    import Gallery from "svelte-image-gallery";

    const placemarkService = getContext("PlacemarkService");
    const dispatch = createEventDispatcher();

    export let placeId;
    export let images = [];

    async function handleClick(e) {
        console.log("deleting " + e.detail.id);
        let confirmDelete = confirm("Do you want to delete this image?");
        if (confirmDelete) {
            await placemarkService.deleteImage(placeId, e.detail.id);
            dispatch("update");
        }
    }

</script>

<Gallery on:click={handleClick}>
    {#each images as image}
        <img id={image.public_id} src="{image.url}"/>
    {/each}
</Gallery>