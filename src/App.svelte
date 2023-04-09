<script>
  import instagram from "$data/instagram.json";

  function numberExtract(str_rep) {
    const lookup = [
      { value: 1, symbol: "" },
      { value: 1e3, symbol: "K" },
      { value: 1e6, symbol: "M" },
      { value: 1e9, symbol: "G" },
      { value: 1e12, symbol: "T" },
      { value: 1e15, symbol: "P" },
      { value: 1e18, symbol: "E" }
    ];
    // regexp extract figures and symbol
    const re = /(\d+(\.\d+){0,1})([kmgtpeKMGTPE]){0,1}/;
    let [full_string, number_string, decimal_string, symbol] = str_rep.match(re);
    // uppercase everything and replace figure with symbols with float number
    let idx = lookup.findIndex((element) => element.symbol == ((symbol===undefined)?"":symbol.toUpperCase()));
    return parseFloat(number_string) * lookup[idx].value;
  }

  /*
    val looks like: ​
      "Avg. Likes": "7.31K"
      Category: "photography"
      "Eng Rate": "0.1%"
      Followers: "580.1M"
      Posts: "7.3K"
      channel_Info: "brand"
      name: "instagram"​
      rank: 1
    and we need floats to be able to graph:
      f_average_like = 7310.0
      f_engagement_rate = 0.1
      f_followers = 580100000.0
      f_posts = 7300.0
  */
  function convertToNumber(val,idx,roArray) {
    // extract and convert what we need for the chart
    return {
      name: val["name"]
      ,f_average_like: numberExtract(val["Avg. Likes"].toString())
      ,f_engagement_rate: numberExtract(val["Eng Rate"].toString())
      ,f_followers: numberExtract(val["Followers"].toString())
      ,f_posts: numberExtract(val["Posts"].toString())
      ,val: val
    };
  }

  let data = instagram.map(convertToNumber);

  console.log(data);

  let width = 800;
  let height = 400;
  const margin = {top: 20, bottom: 20, left: 50, right: 40};

  import { scaleLinear, scaleLog } from "d3-scale";
  import { min, max } from "d3-array";
  import AxeX from "./components/AxeX.svelte";
  import AxeY from "./components/AxeY.svelte";

  let xMin = min(data, d => d.f_posts);
  let xMax = max(data, d => d.f_posts);
  let xName = "Posts";
  const xScale = scaleLog()
    .domain([xMin, xMax])
    .range([0, width - margin.left - margin.right]);

  let yMin = 0;
  let yMax = Math.ceil(max(data, d => d.f_engagement_rate));
  let yName = "Engagement";
  const yScale = scaleLinear()
    .domain([yMin, yMax])
    .range([height - margin.top - margin.bottom, 0]);

  let rMin = min(data, d => d.f_followers);
  let rMax = max(data, d => d.f_followers);
  const rScale = scaleLinear()
    .domain([rMin, rMax])
    .range([4, 30])

// Handle going over and displaying tooltip
  import Tooltip from "./components/Tooltip.svelte";
  let hoveredData = null;

  import Legend from "./components/Legend.svelte";
  let dotLegend = [
    {size: rMin, color: "#ff3e00", text: "~ " + (rMin/1e6).toString()+ " M"}
    ,{size: rMin+(rMax-rMin)/2, color: "#ff3e00", text: "~ " + ((rMin+(rMax-rMin)/2)/1e6).toString()+ " M"}
    ,{size: rMax, color: "#ff3e00", text: "~ " + (rMax/1e6).toString()+ " M"}
  ]

</script>

<h1>Big Brands Engagement Rate according to Posts and Followers</h1>
<svg {width} {height} on:mouseleave={() => {hoveredData = null}} >
  <g class="circles" transform="translate({margin.left} {margin.top})">
    {#each data as account}
      <g id={"graph_data_"+account.name}>
        <circle cx={xScale(account.f_posts)} cy={yScale(account.f_engagement_rate)} r={rScale(account.f_followers)} on:mouseover={() => {hoveredData = account}} on:focus={() => {hoveredData = account}} />
      </g>
    {/each}
  </g>
  <AxeX {width} {height} {xScale} {xMin} {xMax} {margin} {xName}/>
  <AxeY {width} {height} {yScale} {yMin} {yMax} {margin} {yName}/>
</svg>
{#if hoveredData}
<Tooltip account={hoveredData} {xScale} {yScale} {margin} />
{/if}

<Legend {dotLegend} {rScale} />

<style>

svg {
  margin: 0 auto;
}

circle {
  fill: #ff3e00;
  fill-opacity: 0.2;
}

rect {
  fill: #f0f0f0;
}
main {
    text-align: center;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: #f0f0f0;
  }

  h1 {
    font-size: 3rem;
    margin-bottom: 1rem;
    font-weight: 700;
  }

  h2 {
    font-size: 1.5rem;
    color: #333;
    margin-bottom: 2rem;
    line-height: 1.5;
  }

  pre {
    padding: 1px 6px;
    display: inline;
    margin: 0;
    background: #ffb7a0;
    border-radius: 3px;
  }

  a {
    color: #ff3e00;
    text-decoration: inherit;
  }

  footer {
    font-size: 1rem;
    color: #333;
  }
</style>
