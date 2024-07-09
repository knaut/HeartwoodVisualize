<script>
  console.log('PieSlice')

  function getCoordinatesForPercent(percent) {
    const x = Math.cos(2 * Math.PI * percent);
    const y = Math.sin(2 * Math.PI * percent);

    return [x, y]
  }

  const percent = 0.12;

  const startX = getCoordinatesForPercent(0)[0];
  const startY = getCoordinatesForPercent(0)[1];

  const endX = getCoordinatesForPercent(percent)[0];
  const endY = getCoordinatesForPercent(percent)[1];

  const largeArcFlag = percent > .5 ? 1 : 0;

  const pathData = [
    `M ${startX} ${startY}`,
    `A 1 1 0 ${largeArcFlag} 1 ${endX} ${endY}`,
    `L 0 0`
  ].join(' ');

</script>

<div>
  Pie Slice

  <svg
    id="test"
    viewBox="-1 -1 2 2"
    style="transform: rotate(-0.25turn)"
  >
    <path d="{pathData}"></path>
  </svg>

  <svg
    id="test-2"
    viewBox="-1 -1 2 2"
    style="transform: rotate(-0.25turn)"
  >
    <defs>
      <clipPath clipPathUnits="objectBoundingBox" id="clip-path">
        <path d="{pathData}"></path>
      </clipPath>
    </defs>
  </svg>

  <div id="border"></div>
</div>


<style>
  svg {
    border: red 1px solid;
    width: 100px;
  }

  #border {
    background: green;
    height: 100px;
    width: 100px;

/*    border: 5px solid cyan;*/
/*    border-radius: 100%;*/ 
/* these won't work, they conflict with the svg orientation */

    clip-path: url(#clip-path);
  }


</style>
