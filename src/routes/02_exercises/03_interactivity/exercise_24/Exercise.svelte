<script>
  import { json } from "d3-fetch";
  import { onMount } from "svelte";

  import Controls from "../exercise_24/Controls.svelte";
  import Scatterplot from "../exercise_24/Scatterplot.svelte";

  // Load the data
  let data = null;
  let selectedYear = 0;
  let selectedContinent = "all";
  let scatterPlotData = null;

  onMount(async () => {
    data = await json("/data/gapminder.json");
    scatterPlotData = data[0]['countries'];
  });

  function handleData(year, continent) {
    if (data) {
      if (continent == 'all'){
        scatterPlotData = data[year]['countries'];
      } else {
        scatterPlotData = data[year]['countries'].filter(item => item.continent === continent);
      }
    }
  }

  $: handleData(selectedYear, selectedContinent);
</script>

{#if !data}
  <p>Loading the data, please wait...</p>
{:else}
  <div>
    <Scatterplot data = {scatterPlotData} />
    <Controls bind:selectedYear = {selectedYear} bind:selectedContinent = {selectedContinent} />
    <!-- <Scatterplot data={data[0]['countries']} />
    <Controls {data} /> -->
  </div>
{/if}
