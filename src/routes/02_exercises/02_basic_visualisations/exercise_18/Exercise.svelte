<script>
    import {scalelog} from 'd3-scale'
    import {axisBottom} from 'd3-axis'
    import {select} from 'd3-selection'

    const circleRadius = 10;

    // Dimensions
    const width = 800;
    const height = 100;
    const margin = { top: 5, right: 5, bottom: 25, left: 5 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  
    // Array
    const values = [2, 4, 6, 7, 9];

    const scale = scaleLog()
      .domain([1, 10])
      .range([0, innerWidth]);

    const xAxis = (node) => {
      const xAxisFn = axisBottom(scale);
      const axisContainer = select(node);
      xAxisFn(axisContainer);
    };
    
  </script>
  
  <svg viewBox="0 0 {width} {height}">
    <g transform="translate({margin.left},{margin.top})">
      {#each values as value}
        <circle cx = {scale(value)} cy = {innerHeight / 2 - circleRadius} r = {circleRadius} fill = "steelblue"/>
      {/each}
      <g use:xAxis transform = "translate(0, {innerHeight - circleRadius * 2})">
        <text class = "xlabel" x = {width / 2 - margin.left} y = {25} text-anchor = "middle" fill = "currentColor">x label</text>
      </g>
    </g>
  </svg>
  