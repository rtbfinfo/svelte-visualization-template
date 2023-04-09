<script>
    export let account;
    export let xScale;
    export let yScale;
    export let margin;
  
</script>

<div class='tooltip' style="position: absolute;
top: {yScale(account.f_engagement_rate)+margin.bottom}px;
left: {xScale(account.f_posts)+margin.left+20}px">
<h1>{account.name}</h1> 
<p><strong>{account.val["Category"]}</strong></p>
<p>{account.val["Followers"]} Followers</p>
<p>{account.val["Avg. Likes"]} Avg. Likes</p>
</div>

<style>
.tooltip {
    padding: 6px;
    background: white;
    border: 1px solid rgba(0, 0, 0, 0.5);
    pointer-events: none;
    transition: top 300ms ease, left 300ms ease;
}

h1 {
    font-weight: 700;
    margin-bottom: 0.25rem;
}
</style>