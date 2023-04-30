<script>
    import { onMount } from "svelte";
    import Scatterplot from "./Scatterplot.svelte";
    let data;
    let countriesFor1800;
    onMount(async () => {
    const response = await fetch("/data/gapminder.json");
    data = await response.json();
    countriesFor1800 = data.find((entry) => entry.year === '1800').countries;
  });
  </script>
  {#if !countriesFor1800}
    <p>Lo</p>
  {:else}
    <Scatterplot {countriesFor1800} />
  {/if}