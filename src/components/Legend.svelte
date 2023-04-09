<script>
    export let dotLegend;
    export let rScale;

    console.log(dotLegend);
</script>

<svg width=300 height=300>
    <text x=10 y=20 dominant-baseline="hanging">Followers</text>
    <g transform="translate(40 50)">
    {#each dotLegend as dot, index}
        <circle cx=10 cy={index*15+rScale(dot.size)*2} r={rScale(dot.size)} />
        <text x=30 y={index*15+rScale(dot.size)*2}>{dot.text}</text>
    {/each}
    </g>
</svg>

<style>
  svg {
    margin: 0 auto;
  }

  circle {
    fill: #ff3e00;
    fill-opacity: 0.2;
  }
</style>