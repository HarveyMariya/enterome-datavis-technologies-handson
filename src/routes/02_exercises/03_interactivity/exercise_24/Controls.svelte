<script>
  // Properties
  export let selectedYear = 0;
  export let selectedContinent = "all";

  const continents = ["europe", "asia", "americas", "africa"];

  let animationRun = false;

  let timer;

  function handleStart() {
    animationRun = true;
    timer = setInterval(() => {
      const range = selectedYear + 1;
      if (range <= 214) {
        selectedYear = range;
      } else {
        selectedYear = 0;
      }
    }, 50);
  }

  function handleStop() {
    animationRun = false;
    clearInterval(timer);
  }

  function handleReset() {
    handleStop();
    selectedYear = 0;
  }

</script>

<div class="row">
  <div class = "col-4">
    {#if animationRun}
      <button type = "button" class = "btn btn-primary" on:click= {handleStop}> Stop </button>
    {:else}
      <button type="button" class = "btn btn-primary" on:click = {handleStart}> Start </button>
    {/if}
    <button type="button" class = "btn btn-primary" on:click = {handleReset}> Reset </button>
  </div>
  <div class = "col-5">
    <label class = "form-label col-sm-10">
      Year: {1800 + selectedYear}
      <input
        type = "range"
        class = "form-range"
        bind:value = {selectedYear}
        min = {0}
        max = {214}
        step = {1}>
    </label>
  </div>
  <div class = "col-3">
    <select id = "continent-select" class = "form-select" bind:value= {selectedContinent}>
      {#each continents as continent}
        <option value = {continent}>{continent}</option>
      {/each}
    </select>
  </div>
</div>
