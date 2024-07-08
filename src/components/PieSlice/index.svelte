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
    viewBox="-1 -1 2 2"
    style="transform: rotate(-0.25turn)"
  >
    <path d="{pathData}"></path>
  </svg>
</div>


<style>
  svg {
    border: red 1px solid;
  }
</style>
