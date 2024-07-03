<script>
  let constrain = 2;
  let mouse = { x: 0, y: 0 }

  const rings = [
    {
      perspective: 200,
      x: 0,
      y: 0,
      z: 0
    },
    {
      perspective: 200,
      x: 0,
      y: 0,
      z: 50
    }
  ]

  function transform(x, y, el) {
    let box = el.getBoundingClientRect();
    let calcX = -(y - box.y - (box.height / 2)) / constrain;
    let calcY = (x - box.x - (box.width / 2)) / constrain;
    
    for (let i = 0; rings.length > i; i++) {
      rings[i].x = calcX
      rings[i].y = calcY
    }

  }


  function handleMousemove(event) {
    const { clientX, clientY, target } = event

    transform( clientX, clientY, target )
  }

</script>

<div class="container" on:mousemove={handleMousemove}>
  {#each rings as {perspective, x, y, z}, i}
    <div class="ring" id="{`ring-${i}`}" on:mousemove={handleMousemove} style="
      transform: 
        perspective({perspective}px) 
        rotateX({x}deg) 
        rotateY({y}deg)
        translateZ({z}px);
    "></div>
  {/each}
</div>


<style>
  .container {
    height: 100vh;
    width: 100vw;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .ring {
    border: 10px solid red;
    border-radius: 100%;
    height: 200px;
    width: 200px;
    position: absolute;
  }
  .ring:nth-child(2) {
    border: 10px solid cyan;
  }
</style>
