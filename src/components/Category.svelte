<script>
    import {createEventDispatcher} from 'svelte';
    import {slide} from 'svelte/transition';
    import CategoryMap from "./CategoryMap.svelte";

    export let category;

    const dispatch = createEventDispatcher();
    let showMap = false;

    function toggleMap() {
        showMap = showMap === false;
    }

    function deleteMessage() {
        dispatch("delete");
    }
</script>

<div class="box box-link-hover-shadow">
    <h2 class="title">
        {category.name}
    </h2>
    <a href="/#/category/{category._id}" class="button">
      <span class="icon is-small">
        <i class="fas fa-folder-open"></i>
      </span>
    </a>
    <button class="button" on:click={toggleMap}>
        <i class="fas fa-map"></i>
    </button>
    <button class="button" on:click={deleteMessage}>
        <i class="fas fa-trash"></i>
    </button>
    {#if showMap}
        <div class="box" transition:slide>
            <CategoryMap categoryId={category._id} categoryName={category.name}/>
        </div>
    {/if}
</div>