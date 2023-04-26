<script>
  import { scaleLinear, scaleLog, scaleOrdinal } from 'd3-scale';
  import { schemeTableau10 } from 'd3-scale-chromatic';
  import { axisBottom, axisLeft } from 'd3-axis';
  import { max, min } from 'd3-array';
  import { select } from 'd3-selection';

  export let data

    // Dimensions
    const [height, width] = [400, 600];
    const margin = { top: 50, right: 5, bottom: 55, left: 50 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;

    const scaleX = scaleLog()
      .domain([300, 150000])
      .range([0, innerWidth]);
    const scaleY = scaleLinear()
      .domain([0, 90])
      .range([innerHeight, 0]);
    const scaleColor = scaleOrdinal()
      .domain(['europe', 'asia', 'america', 'africa'])
      .range(schemeTableau10);
    const scaleRadius = scaleLinear()
      .domain([min(data.map(item => item.population)), max(data.map(item => item.population))])
      .range([5, 13])
    
    const xAxis = (node) => {
      const xAxisFn = axisBottom(scaleX)
        .tickValues([400, 4000, 40000])
        .tickFormat(value => '$${value');
      xAxisFn(select(node));
    };
    const yAxis = (node) => {
      const yAxisFn = axisLeft(scaleY);
      yAxisFn(select(node));
    }
  </script>
  
  <svg viewBox="0 0 {width} {height}" style="max-width: {width}px">
    <g transform="translate({margin.left}, {margin.top})">
      {#each data as item}
        {#if !(item.income == null || item.life_exp == null)}
          <circle class = "dot" cx = {scaleX(item.income)} cy = {scaleY(item.life_exp)} r = {scaleRadius(item.population)} fill = {scaleColor(item.continent)}>
            <title>{item.country}</title>
          </circle>
        {/if}
      {/each}
      <g use:xAxis transform = "translate(0, {innerHeight})">
        <text x = {(innerWidth / 2)} y = {37} class = "label">GDP per Capital</text>
      </g>
      <g use:yAxis transform = "translate(0, {innerHeight})">
        <text x = {-(innerHeight / 2)} y = {-32} class = "label ylabel">Life Expectancy</text>
      </g>
      <text class = "year" x = {innerWidth} y = {innerHeight - 10}>1800</text>
      <text class = "main" x = {innerWidth / 2} y = {0}> Gapminder Scatterplot</text> 
    </g>
  </svg>

  <style>
    .label {
      font-size: 1.75em;
      fill: currentColor;
      text-anchor: middle;
      vertical-align: bottom;
    }
    .ylabel {
      transform: rotate(-90deg);
    }
    .dot {
      opacity: 0.8;
    }
    .year {
      font-size: 2.25em;
      opacity: 0.4;
      text-anchor: end;
    }
    .main {
      font-size: 2.25em;
      text-anchor: middle;
    }
  </style>
  