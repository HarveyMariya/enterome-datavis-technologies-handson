<script>
    import { onMount } from 'svelte';
    import { json } from 'd3-fetch';
    import Scatterplot from '../exercise_21/Scatterplot.svelte';
   
    let data = null;
    
    onMount(async () => {
        data = await json('/data/gapminder.json');
    });
    </script>
    
    {#if data == null}
    <p>The data is being loaded!</p>
    {:else}
    <Scatterplot data={data[0].countries}/>
    {/if}
      