<script>
  import {scaleLinear as sl,scaleSqrt as ss,scaleOrdinal as so} from "d3-scale";
  import {axisBottom as ab,axisLeft as al} from "d3-axis";
  import {select as s} from "d3-selection";
  import {schemeCategory10 as sc10} from "d3-scale-chromatic";
  import {onMount as om} from "svelte";
  import {max} from "d3-array";
  export let countriesFor1800;
  const [h,w]=[400,600];
  const m={t:50,r:5,b:55,l:50};
  const iW=w-m.l-m.r;
  const iH=h-m.t-m.b;
  const xS=sl().domain([0,max(countriesFor1800,(d)=>+d.income)]).range([0,iW]);
  const yS=sl().domain([0,max(countriesFor1800,(d)=>+d.life_exp)]).range([iH,0]);
  const rS=ss().domain([0,max(countriesFor1800,(d)=>+d.population)]).range([0,25]);
  const cS=so(sc10).domain([...new Set(countriesFor1800.map((d)=>d.continent))]);
  const xA=ab(xS);
  const yA=al(yS);
  const t="Gapminder Visualization (1800)";
  const xAL="Income";
  const yAL="Life Expectancy";
  const yL="1800";
  om(()=>{
    const xAG=s("#x-axis").call(xA);
    const yAG=s("#y-axis").call(yA);
  });
  </script>
  <svg viewBox="0 0 {w} {h}" style="max-width: {w}px">
    <g transform="translate({m.l}, {m.t})">
      <g id="x-axis" transform="translate(0, {iH})"></g>
      <g id="y-axis"></g>
      {#each countriesFor1800 as d}
        <circle
          cx="{xS(+d.income)}"
          cy="{yS(+d.life_exp)}"
          r="{rS(+d.population)}"
          fill="{cS(d.continent)}"
          opacity="0.8"
        ></circle>
      {/each}
      <text x="{iW/2}" y="{iH+m.b/2}" text-anchor="middle" font-size="14">{xAL}</text>
      <text x="{-iH/2}" y="{-m.l/2}" text-anchor="middle" font-size="14" transform="rotate(-90)">{yAL}</text>
      <text x="{iW/2}" y="{-m.t/2}" text-anchor="middle" font-size="18" font-weight="bold">{t}</text>
      <text x="{iW-m.r}" y="{iH-m.b/2}" text-anchor="end" font-size="14">{yL}</text>
    </g>
  </svg>