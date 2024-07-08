<script>

  function getCoordinatesForPercent(percent) {
    const x = Math.cos(2 * Math.PI * percent);
    const y = Math.sin(2 * Math.PI * percent);

    return {x, y}
  }

  const { x, y } = getCoordinatesForPercent(.40);

  console.log(x, y)

/*
  let sectorX = 0.75;
  let sectorY = .096987298;

  function genPath(angle) {
    const radius = 100;
    const radians = (angle * Math.PI) / 180;

    const startX = 100;
    const startY = 100;

    const endX = startX + radius;
    const endY = startY;

    const arcX = startX + radius * Math.cos(radians);
    const arcY = startY - radius * Math.sin(radians);

    const pathData = `
        M ${startX} ${startY} 
        L ${endX} ${endY} 
        A ${radius} ${radius} 0 ${angle > 180 ? 1 : 0} 1 ${arcX} ${arcY} 
        Z
    `;

    return pathData
  }

  const path = genPath(45);
  console.log(path)

  function calcArcPoint(angleInDegrees, centerX, centerY, radius) {
    //polar to cartesian coordinates conversion
    //knowing the value of your angle in degrees..
    //get value of the angle in radians
    console.log(-angleInDegrees * Math.PI / 180.0)
    let angleInRadians = -angleInDegrees * Math.PI / 180.0;
    //get the cartesian x coordinate (centerX = x coordinate of the center of the circle == 250px in our case)
    let x = centerX + radius * Math.cos(angleInRadians);
    //get the cartesian y coordinate (centerY = y coordinate of the center of the circle == 250px in our case)
    let y = centerY + radius * Math.sin(angleInRadians);

    console.log({x, y})

    // return { x, y }
  }
*/

  let constrain = 20;
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

  <svg viewBox="-1 -1 2 2" style="transform: rotate(-0.25turn)">
    <defs>
      <clipPath clipPathUnits="objectBoundingBox" id="sector">
        <!-- <path fill="none" stroke="#111" stroke-width="1" class="sector" d="M0.5,0.5 l0.5,0 A0.5,0.5 0 0,0 0.75, .066987298 z"></path> -->


        <!-- <path d=”M 1 0 A 1 1 0 0 1 0.8 0.59 L 0 0"></path> -->

        <path fill="#ccc" stroke="#111" stroke-width="1" class="sector" d="M0.5,0.5 l0.5,0 A0.5,0.5 0 0,0 {x}, {y} z"></path>



        <!-- <path fill="none" stroke="#111" stroke-width="1" class="sector" d="M0.5,0.5 l0.5,0 A0.5,0.5 0 0,0 {sectorX}, {sectorY} z"></path> -->
        <!-- <path fill="none" stroke="#111" stroke-width="1" class="sector" d="M0.5,0.5 l0.5,0 A0.5,0.5 0 0,0 100, 0 z"></path> -->
      </clipPath>
    </defs>
  </svg>

  <!-- <svg width="0" height="0">
      <clipPath id="clipPath">
          <path fill="blue" id="clipPathPath" d="{path}" />
      </clipPath>
  </svg> -->
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
    clip-path: url(#sector);
  }
  .ring:nth-child(2) {
    border: 10px solid cyan;
  }
</style>
