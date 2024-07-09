<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  console.log('PieChart', d3);

  onMount(() => {

    d3.select('#demo5')
      .append('g')
      .attr('transform', 'translate(200,200)');

    d3.select('#demo6')
      .append('g')
      .attr('transform', 'translate(200,200)');

    const angleGen = d3.pie()
      .startAngle(Math.PI / 4);

    const data = angleGen([10, 10, 10, 10, 40, 20, 20, 40]);
    const data2 = angleGen([1]);

    const arcGen = d3.arc()
      .innerRadius(30)
      .outerRadius(90);

    d3.select("#demo5 g")
      .selectAll('path')
      .data(data)
      .enter()
      .append('path')
      .attr('d', arcGen)
      .attr('fill', 'pink')
      .attr('stroke', 'gray')
      .attr('stroke-width', 1);

    const arcGen2 = d3.arc()
      .innerRadius(95)
      .outerRadius(130);

    d3.select("#demo6 g")
      .selectAll('path')
      .data(data2)
      .enter()
      .append('path')
      .attr('d', arcGen2)
      .attr('fill', 'pink')
      .attr('stroke', 'gray')
      .attr('stroke-width', 1)


  })

</script>


<div>
  Pie Chart

  <svg id="demo5" width="400" height="400"></svg>
  <svg id="demo6" width="400" height="400"></svg>
</div>

<style>
  svg {
    border: 1px solid red;
    position: absolute;
  }
</style>

