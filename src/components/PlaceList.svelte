<script>
    import placeholder from "../assets/placeholder.jpg";

    export let placeList;
</script>
<div class="container">
    <div class="columns is-multiline">
        {#each placeList as place}
            <div class="column is-one-quarter">
                <div class="card">
                    <header class="card-header">
                        <p class="card-header-title">
                            {place.name}
                        </p>
                    </header>
                    <div class="card-image">
                        <figure class="image is-4by3">
                            {#if place.img[0]}
                                <img alt="Cover image" src={place.img[0].url}>
                            {:else}
                                <img alt="Default image" src={placeholder}>
                            {/if}
                        </figure>
                    </div>
                    <div class="card-content">
                        <div class="content">
                            {place.description}
                        </div>
                        <div class="content">
                            {place.location.latitude} , {place.location.longitude}
                        </div>
                    </div>
                    <footer class="card-footer">
                        <a onclick="" class="card-footer-item">
                            <i class="fas fa-image"></i>
                        </a>
                        <a href="" class="card-footer-item">
                            <i class="fas fa-trash"></i>
                        </a>
                    </footer>
                </div>
            </div>
        {/each}
    </div>
</div>