<script>
	import * as d3 from "d3";

	import Line from './Line.svelte';
	import XAxis from './XAxis.svelte';
	import GridLines from './GridLines.svelte';
	import Crosshair from './Crosshair.svelte';
	import Point from './Point.svelte';
	
	export let stats;

	let hoveredPoint = null;

	const margin = {
    top: 50,
    right: 25,
    bottom: 30,
    left: 70,
  };

	let width = 100;
  $: height = 300;

  $: innerWidth = width - margin.left - margin.right;
  $: innerHeight = height - margin.top - margin.bottom;

	const xAccessor = d => d.year;
  const yAccessor = d => d.population;

	const bisectX = d3.bisector(xAccessor).left;

	$: xScale = d3
    .scaleLinear()
    .domain(d3.extent(stats, xAccessor))
    .range([0, innerWidth]);

	$: yScale = d3
    .scaleLinear()
    .domain(d3.extent(stats, yAccessor))
    .range([innerHeight, 0])
    .nice();

	$: xAccessorScaled = d => xScale(xAccessor(d));
  const yAccessorScaled = d => yScale(yAccessor(d));

	const handleMouseMove = event => {
    const xCoordinate = xScale.invert(event.offsetX - margin.left);
    const index = bisectX(stats, xCoordinate);
    hoveredPoint = stats[index - 1];
  };

  const handleMouseLeave = () => {
    hoveredPoint = null;
  };
</script>

<div 
	class="wrapper" 
	bind:clientWidth={width}
>
  <svg 
		role="img"
    aria-label="line chart that shows the variation of Afghanistan's population between 2000 and 2022" 
		{width} 
		{height}
		on:mousemove={handleMouseMove}
    on:mouseleave={handleMouseLeave}
	>
    <g transform={`translate(${margin.left}, ${margin.top})`}>
		  <XAxis
        {xScale}
				{innerHeight}
				{hoveredPoint}
        label="Year"
      />
			<GridLines
        {yScale}
				{innerWidth}
				{hoveredPoint}
        label="Albania"
      />
      <Line 
				{stats}
				{xAccessorScaled}
				{yAccessorScaled}
			/>
			{#if hoveredPoint}
				<Crosshair
          xAccessorScaled={xAccessorScaled(hoveredPoint)}
          yAccessorScaled={yAccessorScaled(hoveredPoint)}
          xLabel={xAccessor(hoveredPoint)}
          yLabel={yAccessor(hoveredPoint)}
					{innerHeight}
        />
        <Point
          x={xAccessorScaled(hoveredPoint)}
          y={yAccessorScaled(hoveredPoint)}
          color="#0f8677"
        />
      {/if}
    </g>
  </svg>
</div>

<style>
  .wrapper {
    position: relative;
    width: 100%;
  }
</style>
