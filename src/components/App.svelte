<script>
  import * as d3 from 'd3';
  import * as topojson from 'topojson-client';

  let path;
  let us;

  // Fetch the JSON data
  fetch('path/to/counties-albers-10m.json')
    .then(response => response.json())
    .then(data => {
      us = data;
      path = d3.geoPath();
    })
    .catch(error => console.error('Error fetching the JSON data:', error));
</script>

<main>
  <svg viewBox="0 0 975 610">
    <g fill="none" stroke="#000" stroke-linejoin="round" stroke-linecap="round">
      {#if us}
        <path stroke="#aaa" stroke-width="0.5" d="{path(topojson.mesh(us, us.objects.counties, (a, b) => a !== b && (a.id / 1000 | 0) === (b.id / 1000 | 0)))}"></path>
        <path stroke-width="0.5" d="{path(topojson.mesh(us, us.objects.states, (a, b) => a !== b))}"></path>
        <path d="{path(topojson.feature(us, us.objects.nation))}"></path>
      {/if}
    </g>
  </svg>
</main>

<style>
  /* Write your CSS here */
</style>
