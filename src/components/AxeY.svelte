<script>
    export let width;
    export let height;
    export let yScale;
    export let yMin;
    export let yMax;
    export let margin;
    export let yName;

    let Yticks = [yMin, Math.round(yMin+(yMax-yMin)/4*1*100)/100, Math.round(yMin+(yMax-yMin)/4*2*100)/100, Math.round(yMin+(yMax-yMin)/4*3*100)/100, yMax];
</script>

<g transform="translate({margin.left} {margin.top})">
    {#each Yticks as tick}
    <text x="0" y={yScale(tick)} dy=-6 dx={-margin.left}>{tick}</text>
    <line x1="0" y1={yScale(tick)} x2={width} y2={yScale(tick)} stroke={tick==yMin?"black":"#cecece"} ></line>
    {/each}
    <text x={margin.left} y={margin.top} text-anchor="middle" dominant-baseline="hanging" transform="rotate(-90) translate(-80 -10)">{yName}</text>
</g>