<script>
  import { csv } from "d3-fetch";
  import { onMount } from "svelte";
  import Scatter from "./Scatterplot.svelte";

  // Load the data
  let data = null;
  let selected = null;
  let selectedS1 = [];
  let selectedS2 = [];
 
  onMount(async () => {
    data = await csv("/data/cars-2.csv");
    selected = data ? data.map(() => true) : [];
  });

  // Configurations
  const s1 = {
    x: (d) => +d.Horsepower,
    y: (d) => +d.Acceleration,
    xLabel: "Horsepower",
    yLabel: "Acceleration",
  };

  const s2 = {
    x: (d) => +d.Displacement,
    y: (d) => +d.Miles_per_Gallon,
    xLabel: "Displacement",
    yLabel: "Miles per Gallon",
  };

  function handleSelected(selectedS1, selectedS2) {
    if (data) {
      if (selectedS1.length) {
        selected = selectedS1;
      } else if (selectedS2.length) {
        selected = selectedS2;
      } else {
        selected = data.map(() => true);
      }
    }
  }

  $: handleSelected(selectedS1, selectedS2);
</script>

{#if !data}
  <p>Loading the data, please wait...</p>
{:else}
  <!-- <div class="d-flex justify-content-around"> -->
  <Scatter {data} {...s1} bind:localSelected={selectedS1} selected= {selected}/>
  <Scatter {data} {...s2} bind:localSelected={selectedS2} selected= {selected}/>
  <!-- </div> -->
{/if}
