<script>
  import { extent } from "d3-array";
  import { select } from "d3-selection";
  import { scaleLinear } from "d3-scale";
  import { axisBottom, axisLeft } from "d3-axis";
  import {brush} from "d3-brush";
  import {onMount} from "svelte";

  // Properties
  export let data = [];
  export let x = (d) => d.x;
  export let y = (d) => d.y;
  export let xLabel = "x";
  export let yLabel = "y";
  export let selected = data.map(() => true);
  export let localSelected = [];

  // Dimensions
  const width = 300;
  const height = 230;
  const margin = { top: 10, right: 10, bottom: 35, left: 45 };
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Scales
  const xScale = scaleLinear().domain(extent(data, x)).range([0, innerWidth]);
  const yScale = scaleLinear().domain(extent(data, y)).range([innerHeight, 0]);

  // Axes
  const xAxis = (node) => axisBottom(xScale)(select(node));
  const yAxis = (node) => axisLeft(yScale)(select(node));

  let canvas;
  // Brushing
  // The range of the selection rectangle.
  // If there is no active selection, it contains: null,
  // otherwise, it contains: [ [x0, y0], [x1, y1] ]
  let range = null;
  let coords = data.map(d => {
    return {
      x: xScale(x(d)),
      y: yScale(y(d)),
    };
  });

  onMount(() => {
    // initialize brush
    select(canvas).call(brush().on('start brush end', ({ selection }) => {
      if (selection) {
        range = selection;
      } else {
        range = null;
      }
    }));
  });

  function handleRange(range) {
    if (coords) {
      if (range) {
        const [[x0, y0], [x1, y1]] = range;
        localSelected = coords.map(d => x0 <= d.x && d.x < x1 && y0 <= d.y && d.y < y1);
      } else {
        localSelected = [];
      }
    }
  }

  $: handleRange(range);
</script>

<svg viewBox="0 0 {width} {height}" class="mx-2" style="max-width: {width}px">
  <g transform="translate({margin.left},{margin.top})">
    <g bind:this={canvas}>
      {#each coords as d, i}
        <circle
          cx={d.x}
          cy={d.y}
          r={3}
          fill={selected[i] ? "steelblue": "darkgrey"}
          fill-opacity={selected[i] ? 0.5 : 0.3}
          stroke={selected[i] ? "steelblue": "darkgrey"}
          stroke-width="1.5"
          stroke-opacity={selected[i] ? 1 : 0.5}
        />
      {/each}
    </g>
    <g use:xAxis transform="translate(0, {innerHeight})">
      <text class="axisLabel" x={innerWidth / 2} y={margin.bottom - 5}
        >{xLabel}</text
      >
    </g>
    <g use:yAxis>
      <text
        class="axisLabel"
        y={-margin.left + 10}
        x={-innerHeight / 2}
        transform="rotate(-90)">{yLabel}</text
      >
    </g>
  </g>
</svg>

<style>
  .axisLabel {
    text-anchor: middle;
    vertical-align: bottom;
    fill: currentcolor;
  }
</style>
