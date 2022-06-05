<script>
    import Chart from 'svelte-frappe-charts';
    import {getContext, onMount} from "svelte";

    const placemarkService = getContext("PlacemarkService");

    let chartType = "bar";
    let trend;
    let unique = {};

    let averageDailyTemperatures = {
        labels: ["Today", "Tomorrow", "Next day"],
        datasets: [
            {
                values: []
            }
        ]
    };

    async function calculateAverageDailyTemperatures() {
        const allPlaces = await placemarkService.getAllPlaces();
        const totalPlaces = allPlaces.length;
        let todaySum = 0;
        let tomorrowSum=0;
        let nextDaySum = 0;
        allPlaces.forEach(place => {
            todaySum += place.temperature;
            tomorrowSum += place.tomorrowDayTemp;
            nextDaySum += place.nextdayDayTemp;
        });
        const todayAverage = todaySum / totalPlaces;
        const tomorrowAverage = tomorrowSum / totalPlaces;
        const nextdayAverage = nextDaySum / totalPlaces;
        averageDailyTemperatures.datasets[0].values.push(todayAverage);
        averageDailyTemperatures.datasets[0].values.push(tomorrowAverage);
        averageDailyTemperatures.datasets[0].values.push(nextdayAverage);
        if (nextdayAverage > tomorrowAverage && tomorrowAverage > todayAverage) {
            trend = "Increasing"
        }
        else if (nextdayAverage < tomorrowAverage && tomorrowAverage < todayAverage) {
            trend = "Decreasing"
        }
        else {
            trend = "None"
        }
    }

    onMount(async () => {
        await calculateAverageDailyTemperatures();
        unique = {};
    });

    function switchChartTypeToLine() {
        chartType = "line";
        unique = {};
    }

    function switchChartTypeToBar() {
        chartType = "bar";
        unique = {};
    }
</script>

<h1 class="title is-4">Average Daily Temperatures</h1>
<h2 class="subtitle">The trend of forecasted average daily temps across all places is: {trend}</h2>
{#key unique}
    <Chart data={averageDailyTemperatures} type={chartType}/>
{/key}
<button class="button" on:click={switchChartTypeToBar}>
    Bar
</button>
<button class="button" on:click={switchChartTypeToLine}>
    Line
</button>