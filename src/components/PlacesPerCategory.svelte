<script>
    import Chart from 'svelte-frappe-charts';
    import {get} from 'svelte/store';
    import {user} from "../stores.js"
    import {getContext, onMount} from "svelte";

    const placemarkService = getContext("PlacemarkService");

    let chartType = "bar";
    let unique = {};

    let placesPerCategory = {
        labels: [],
        datasets: [
            {
                values: []
            }
        ]
    };

    async function populatePlacesPerCategory() {
        placesPerCategory.labels = [];
        const allCategories = await placemarkService.getAllCategories();
        const storedUser = get(user);
        const categoryList = allCategories.filter(category => category.userid === storedUser.userId);
        const allPlaces = await placemarkService.getAllPlaces();
        let categoryCount = 0;
        categoryList.forEach(category => {
            let categoryPlaces = allPlaces.filter(place => place.categoryid === category._id);
            placesPerCategory.labels.push(category.name);
            placesPerCategory.datasets[0].values[categoryCount] = categoryPlaces.length;
            categoryCount++;
        });
    }

    onMount(async () => {
        await populatePlacesPerCategory();
    });

    function switchChartTypeToPie() {
        chartType = "pie";
        unique = {};
    }

    function switchChartTypeToBar() {
        chartType = "bar";
        unique = {};
    }
</script>

<h1 class="title is-4">PlacesPerCategory</h1>
{#key unique}
    <Chart data={placesPerCategory} type={chartType}/>
{/key}
<button class="button" on:click={switchChartTypeToBar}>
    Bar
</button>
<button class="button" on:click={switchChartTypeToPie}>
    Pie
</button>