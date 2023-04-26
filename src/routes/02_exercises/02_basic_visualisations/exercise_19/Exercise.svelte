<script>
  import { scaleLinear, scaleOrdinal } from "d3-scale";
  import { extent } from 'd3-array';
  import { schemeDark2 } from 'd3-scale-chromatic';

    const circleRadius = 5;


    // Dimensions
    const width = 800;
    const height = 100;
    const margin = { top: 20, right: 5, bottom: 5, left: 5 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  
    // Array
    const values = [
      { x: 2, y: 1, category: "cat1" },
      { x: 4, y: 2, category: "cat3" },
      { x: 6, y: 1, category: "cat2" },
      { x: 7, y: 3, category: "cat3" },
      { x: 9, y: 1, category: "cat2" }
    ];

    const scaleX = scaleLinear()
      .domain([0, 10])
      .range([0, innerWidth]);
    const scaleY = scaleLinear()
      .domain([0, 3])
      .range([innerHeight, 0]);
    const scaleColor = scaleOrdinal()
      .domain(extent(values.map(value => value.category)))
      .range(schemeDark2);
  </script>
  
  <svg viewBox="0 0 {width} {height}">
    <g transform="translate({margin.left},{margin.top})">
      {#each values as value}
        <line x1= {scaleX(value.x)} x2 = {scaleX(value.x)} y1 = {scaleY(value.y)} y2 = {scaleY(0)} stroke = "black" />
        <circle cx ={scaleX(value.x)} cy = {scaleY(value.y)} r = {circleRadius} fill = {scaleColor(value.category)} />
        <text x = {scaleX(value.x)} y = {scaleY(value.y) - 10} > {value.y}</text>
      {/each}
      <line x1 = {0} x2 = {innerWidth} y1 = {innerHeight} y2 = {innerHeight} stroke = "black" />
    </g>
  </svg>
  
  <style>
    text {
      text-anchor: middle;
      font-size: small;
    }
  </style>
  