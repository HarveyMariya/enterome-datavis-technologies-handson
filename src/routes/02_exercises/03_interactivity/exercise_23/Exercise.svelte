<script>
    // Dimensions
    const width = 800;
    const height = 100;
    const margin = { top: 10, right: 20, bottom: 20, left: 20 };
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  
    // Arrays
    const points = [
      innerWidth / 2 - 60,
      innerWidth / 2 - 30,
      innerWidth / 2,
      innerWidth / 2 + 30,
      innerWidth / 2 + 60
    ];

    const rPoints = 10;
    const yPoints = innerHeight / 2;
  
    // All lights with a higher index are on!
    let index = points.length;
  
    // Color
    let color = "darkred";

    const interval = setInterval(() => {
      index -= 1;
    }, 1000)

    function handleInterval(id) {
      if (id < 0) {
        clearInterval(interval);
        color = "darkgreen";
      }
    }

    $: handleInterval(index);
  </script>
  
  <svg viewBox="0 0 {width} {height}">
    <g transform="translate({margin.left},{margin.top})">
      {#each points as point, i}
        <circle
          cx = {point}
          cy = {yPoints}
          r = {rPoints}
          fill = {color}
          opacity = {i >= index ? 1 : 0.3}>
        </circle>
      {/each}
    </g>
  </svg>
  