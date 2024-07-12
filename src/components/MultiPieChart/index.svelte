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
  ]

  console.log('Multi-level Pie Chart', d3)

  onMount(() => {
   
    for (let i = 0; skillData.length > i; i++) {

      const { skill, duration, fill } = skillData[i]

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

          const tooltip = d3.select(`#demo${i + 1}-${c + 1}`)
            .append('div')
            .style('position', 'absolute')
            // .style('visibility', 'hidden')
            .text(skill)

          d3.select(`#demo${i + 1}`)
            .on('mouseover', function() {
              return tooltip.style('visibility', 'visible')
            })
            .on('mousemove', function(event) {
              console.log(event)
              return tooltip.style('top', (event.pageY - 100) + 'px').style('left', (event.pageX - 100) + 'px')
            })
            .on('mouseout', function() {
              return tooltip.style('visibility', 'hidden')
            })
          
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

        const tooltip = d3.select(`#demo${i + 1}`)
          .append('div')
          .style('position', 'absolute')
          // .style('visibility', 'hidden')
          .text(skill)

          console.log(tooltip)

        d3.select(`#demo${i + 1}`)
          .on('mouseover', function() {
            return tooltip.style('visibility', 'visible')
          })
          .on('mousemove', function(event) {
            return tooltip.style('top', (event.pageY - 100) + 'px').style('left', (event.pageX - 100) + 'px')
          })
          .on('mouseout', function() {
            return tooltip.style('visibility', 'hidden')
          })

      }

    }
    

  })

</script>


<div class="container"  style="
  border: 1px solid blue;
">
  <div class="svg-wrapper" on:mousemove={handleMousemove} style="
  transform:
    perspective(100px)
    rotateX({perspX}deg)
    rotateY({perspY}deg)
    translateZ(0px);
  ">
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
</div>

<style>
  svg {
    border: 1px solid red;
    position: absolute;
  }

  .svg-wrapper {
    position: relative;
    height: 400px;
    width: 400px;
  }

  .container {
    width: 400px;
    height: 400px;
    /*height: 100vh;
    width: 100vw;
    display: flex;
    align-items: center;
    justify-content: center;*/
  }
</style>
