<script>
  import { scaleLinear, scaleBand } from 'd3-scale';
  import { axisBottom, axisLeft } from 'd3-axis';
  import { select } from 'd3-axis';

  // Dimensions
  const width = 600;
  const height = 300;
  const margin = { top: 10, right: 10, bottom: 30, left: 60 };
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Array
  const data = [
    { service: "Netflix", viewers: 2.9 },
    { service: "Amazon Prime Video", viewers: 1.3 },
    { service: "Disney+", viewers: 2.1 },
    { service: "Hulu", viewers: 0.9 },
    { service: "Apple TV", viewers: 1.1 },
    { service: "Rakuten", viewers: 0.4 }
  ];

  const scaleX = scaleBand()
    .domain(data.map((item) => item.service))
    .range([0, innerWidth])
    .padding(0.20);
  const scaleY = scaleLinear()
    .domain([0, 3])
    .range([innerHeight, 0]);
  const scaleBar = scaleLinear()
    .domain([0, 3])
    .range([0, innerHeight]);

  const xAxis = (node) => {
    const xAxisFn = axisBottom(scaleX);
    xAxisFn(select(node));
  };

  const yAxis = (node) => {
    const yAxisFn = axisLeft(scaleY);
    yAxisFn(select(node));
  };
</script>
  
<!-- setting a viewBox and max-width allows the SVG to shrink but not grow! -->
<svg viewbox="0 0 {width} {height}" style="max-width: {width}px">
  <g transform={`translate(${margin.left},${margin.top})`}>
    {#each data as item}
      <rect x ={scaleX(item.service)} y = {scaleY(item.viewers)} width = {scaleX.bandwidth()} height = {scaleBar(item.viewers)} fill = "steelblue" />
    {/each}
    <g use:xAxis transform = "translate(0, {innerHeight})" />
    <g use:yAxis>
      <text x = {-(innerHeight / 2)} y = {-(margin.left / 2)} class = "ylabel">Viewers (Million)</text>
    </g>
  </g>
</svg>
  
<style>
  .ylabel {
    text-anchor: middle;
    transform: rotate(-90deg);
    fill: currentColor;
  }
</style>