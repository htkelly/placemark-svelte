<script>
    import TitleBar from "../components/TitleBar.svelte";
    import MainNavigator from "../components/MainNavigator.svelte";
    import PlacesPerCategory from "../components/PlacesPerCategory.svelte";
</script>

<div class="columns is-vcentered">
    <div class="column is-two-thirds">
        <TitleBar subTitle={"Check out your analytics"} title={"PlaceMark Analytics"}/>
    </div>
    <div class="column">
        <MainNavigator/>
    </div>
</div>

<PlacesPerCategory/>
