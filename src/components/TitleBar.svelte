<script>
    import globe from "../assets/globe.png";
    import {user} from "../stores.js"

    export let title = "";
    export let subTitle = "";
</script>

<div class="box has-text-centered columns m-2">
    <div class="column">
        <img width="60" src="{globe}" alt="img"/>
    </div>
    <div class="column">
        <div class="title is-5"> {title} </div>
        <div class="subtitle is-5"> {subTitle} </div>
    </div>
    <div class="column">
        <i class="fas fa-map-marker fa-3x" style="color:rgb(95, 96, 173)"></i>
        {#if $user.email}
            <div class="is-size-7">{$user.email} </div>
        {:else}
            <div class="is-size-7">PlaceMark-Svelte 0.1</div>
        {/if}
    </div>
</div>
