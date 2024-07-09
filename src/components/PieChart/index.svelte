<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  console.log('PieChart', d3);

  onMount(() => {

    d3.select('#demo4')
      .append('g')
      .attr('transform', 'translate(100, 100)');

    const data = [
      { startAngle: 0, endAngle: Math.PI/4 },
      { startAngle: Math.PI/4, endAngle: Math.PI/2 },
      { startAngle: Math.PI/2, endAngle: Math.PI },
      { startAngle: Math.PI, endAngle: 2*Math.PI }
    ]

    const arcGen = d3.arc()
      .innerRadius(70)
      .outerRadius(90);

    d3.select('#demo4 g')
      .selectAll('path')
      .data(data)
      .enter()
      .append('path')
      .attr('d', arcGen)
      .attr('fill', 'pink')
      .attr('stroke', 'gray')
      .attr('stroke-width', 1)


  })

</script>


<div>
  Pie Chart

  <svg id="demo4" width="200" height="200"></svg>
</div>

