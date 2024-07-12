<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  let constrain = 50;
  let perspX = 0;
  let perspY = 0;

  function transform(x, y, el) {
    let box = el.getBoundingClientRect();
    let calcX = -(y - box.y - (box.height / 2)) / constrain;
    let calcY = (x - box.x - (box.width / 2)) / constrain;

    perspX = calcX
    perspY = calcY
  }

  function handleMousemove(event) {
    const { clientX, clientY, target } = event

    transform( clientX, clientY, target )
  }

  const skillData = [
    { skill: 'CSS', duration: [0, 11], fill: '#012030' },
    { skill: 'HTML', duration: [4, 12], fill: '#13678A' },
    { skill: 'JS', duration: [[6, 9], [11, 12]], fill: '#45C4B0' },
    { skill: 'Svelte', duration: [0, 11], fill: '#9AEBA3' },
    { skill: 'Rust', duration: [[2, 3], [5,6]], fill: '#DAFDBA' },
    { skill: 'TypeScript', duration: [[9, 10], [11,11.5]], fill: '#DAFDBA' },
  ]

  onMount(() => {
   
    for (let i = 0; skillData.length > i; i++) {

      const { skill, duration, fill } = skillData[i]

      if (Array.isArray(duration[0])) {

        for (let c = 0; duration.length > c; c++) {

          d3.select(`#demo${i + 1}-${c + 1}`)
            .append('g')
            .attr('transform', 'translate(0, 0)');
          
          const angleGen = d3.pie()
            .startAngle( duration[c][0] * Math.PI / 6 )
            .endAngle( duration[c][1] * Math.PI / 6 ); // need to convert months to angles properly

          const data = angleGen([1]);

          const arcGen = d3.arc()
            .innerRadius((i + 1 / (i + 1.1)) * 25)
            .outerRadius((i + 1 / (i + 1)) * 30);

          d3.select(`#demo${i + 1}-${c + 1} g`)
            .selectAll('path')
            .data(data)
            .enter()
            .append('path')
            .attr('d', arcGen)
            .attr('fill', fill)
            .attr('stroke', 'gray')
            .attr('stroke-width', 1)
          
        }

      } else {

        d3.select(`#demo${i + 1}`)
          .append('g')
          .attr('transform', 'translate(0, 0)');

        const angleGen = d3.pie()
          .startAngle( duration[0] * Math.PI / 6 )
          .endAngle( duration[1] * Math.PI / 6 ); // need to convert months to angles properly
        
        const data = angleGen([1]);
        
        const arcGen = d3.arc()
          .innerRadius((i + 1 / (i + 1.1)) * 25)
          .outerRadius((i + 1 / (i + 1)) * 30);
        
        d3.select(`#demo${i + 1} g`)
          .selectAll('path')
          .data(data)
          .enter()
          .append('path')
          .attr('d', arcGen)
          .attr('fill', fill)
          .attr('stroke', 'gray')
          .attr('stroke-width', 1);

      }

    }
    

  })

</script>



  <div class="svg-wrapper">
    {#each skillData as skill, i}
      
      {#if Array.isArray(skill.duration[0])}

        {#each skill.duration as duration, c}
          <!-- might have to scale viewboxes based on number of data -->
          <!-- viewbox args, center is half of width/height -->
          <svg id="demo{i + 1}-{c + 1}" viewBox="-175 -175 350 350"></svg>        
        {/each}
      
      {:else}
        <svg id="demo{i + 1}" viewBox="-175 -175 350 350"></svg>  
      {/if}
      
    {/each}
  </div>

<style>
  :global(body) {
    padding: 0 !important;
  }

  svg {
    border: 1px solid red;
    position: absolute;
  }

  .svg-wrapper {
    position: relative;
    border: 1px solid cyan;
/*    width: 100%;*/
    width: 100vh;
    height: 100vh;
  }
</style>
