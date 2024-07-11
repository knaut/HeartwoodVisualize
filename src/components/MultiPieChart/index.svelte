<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  const skillData = [
    { skill: 'CSS', duration: [0, 11] },
    { skill: 'HTML', duration: [4, 12] },
    { skill: 'JS', duration: [[6, 9], [11, 12]] },
    { skill: 'Svelte', duration: [0, 11] },
  ]

  console.log('Multi-level Pie Chart', d3)

  onMount(() => {
   

    // const skillData = [
    //   { skill: 'CSS', duration: [0, 11] },
    //   { skill: 'HTML', duration: [4, 12] },
    //   { skill: 'JS', duration: [[6, 9], [11, 12]] },
    //   { skill: 'Svelte', duration: [0, 11] },
    // ]

    
    for (let i = 0; skillData.length > i; i++) {

      const { skill, duration } = skillData[i]

      if (Array.isArray(duration[0])) {

        for (let c = 0; duration.length > c; c++) {

          d3.select(`#demo${i + 1}-${c + 1}`)
            .append('g')
            .attr('transform', 'translate(200,200)');
          
          const angleGen = d3.pie()
            .startAngle( duration[c][0] * Math.PI / 6 )
            .endAngle( duration[c][1] * Math.PI / 6 ); // need to convert months to angles properly

          const data = angleGen([1]);

          const arcGen = d3.arc()
            .innerRadius((i + 1 / (i+1)) * 30)
            .outerRadius((i + 1 / (i+2)) * 40);

          d3.select(`#demo${i + 1}-${c + 1} g`)
            .selectAll('path')
            .data(data)
            .enter()
            .append('path')
            .attr('d', arcGen)
            .attr('fill', 'pink')
            .attr('stroke', 'gray')
            .attr('stroke-width', 1);
          
        }

      } else {

        d3.select(`#demo${i + 1}`)
          .append('g')
          .attr('transform', 'translate(200,200)');

        const angleGen = d3.pie()
          .startAngle( duration[0] * Math.PI / 6 )
          .endAngle( duration[1] * Math.PI / 6 ); // need to convert months to angles properly
        
        const data = angleGen([1]);
        
        const arcGen = d3.arc()
          // .innerRadius(30)
          // .outerRadius(90);
          .innerRadius((i + 1 / (i+1)) * 30)
          .outerRadius((i + 1 / (i+2)) * 40);
        
        d3.select(`#demo${i + 1} g`)
          .selectAll('path')
          .data(data)
          .enter()
          .append('path')
          .attr('d', arcGen)
          .attr('fill', 'pink')
          .attr('stroke', 'gray')
          .attr('stroke-width', 1);

      }

    }
    

  })

</script>


<div>
  
  {#each skillData as skill, i}
    
    {#if Array.isArray(skill.duration[0])}

      {#each skill.duration as duration, c}
        <svg id="demo{i + 1}-{c + 1}" width="400" height="400"></svg>        
      {/each}
    
    {:else}
      <svg id="demo{i + 1}" width="400" height="400"></svg>  
    {/if}
    
  {/each}

</div>

<style>
  svg {
    border: 1px solid red;
    position: absolute;
/*    transform: rotate(-90deg);*/
  }
</style>
