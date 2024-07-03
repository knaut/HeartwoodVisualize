<script>
  let constrain = 20;
  let mouse = { x: 0, y: 0 }
  let ring;

  function transform(x, y, el) {
    let box = el.getBoundingClientRect();
    let calcX = -(y - box.y - (box.height / 2)) / constrain;
    let calcY = (x - box.x - (box.width / 2)) / constrain;
    
    return "perspective(100px) "
      + "   rotateX("+ calcX + "deg) "
      + "   rotateY("+ calcY + "deg) ";
  }

  let styleString;

  function handleMousemove(event) {
    mouse.x = event.clientX;
    mouse.y = event.clientY;

    // console.log(mouse)

    styleString = transform(mouse.x, mouse.y, event.target)
  }


</script>

<div class="container" on:mousemove={handleMousemove}>
  <div class="ring" bind:this={ring} style="
    transform: {styleString}
  "></div>  
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
    border: 2px solid red;
    border-radius: 100%;
    height: 100px;
    width: 100px;
    position: absolute;
  }
</style>
