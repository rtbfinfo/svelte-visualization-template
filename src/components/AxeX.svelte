<script>
    export let height;
    export let width;
    export let xScale;
    export let xMin;
    export let xMax;
    export let margin;
    export let xName;

    let Xticks = [xMin, Math.round((xMax-xMin)/1000), Math.round((xMax-xMin)/100), Math.round((xMax-xMin)/10), xMax];
    console.log(Xticks);
</script>

<g>
    <text x={width-margin.right-60} y={height-margin.bottom+5} text-anchor="middle" dominant-baseline="hanging">{xName}</text>
    {#each Xticks as tick}
    <text x={xScale(tick)+margin.left} y={height - margin.bottom} dy="6" text-anchor="middle" dominant-baseline="hanging">{tick}</text>
    <line x1={xScale(tick)+margin.left} y1={height - margin.bottom} x2={xScale(tick)+margin.left} y2="0" stroke="#cecece"></line>
    {/each}
</g>