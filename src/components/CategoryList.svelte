<script>
    import {createEventDispatcher, getContext} from 'svelte';
    import Category from "./Category.svelte";

    const placemarkService = getContext("PlacemarkService");
    const dispatch = createEventDispatcher();

    export let categoryList;

    async function deleteCategory(id) {
        await placemarkService.deleteCategory(id);
        dispatch("update");
    }

</script>

{#each categoryList as category (category._id)}
    <Category category={category} on:delete={deleteCategory(category._id)}/>
{/each}
