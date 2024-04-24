<script>
	export let xScale;

	const numberOfTicks = (pixelsAvailable, pixelsPerTick = 140) =>
    Math.floor(Math.abs(pixelsAvailable) / pixelsPerTick);

  $: [xMin, xMax] = xScale.range();

  $: ticks = xScale.ticks(numberOfTicks(xMax - xMin));
</script>
