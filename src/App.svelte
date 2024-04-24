<script>
  import { stats } from './data';
	import LineChart from './LineChart.svelte';
</script>

<div class="app">
	<LineChart {stats} />
</div>

<style>
	@import url("https://fonts.googleapis.com/css?family=Merriweather:700&display=swap");
	@import url("https://fonts.googleapis.com/css?family=Lato:400&display=swap");
	
	:global(body) {
		background: #f2f2f2;
    color: black;
		font-family: "Lato", sans-serif;
		font-size: 12px;
    font-weight: 100;
    line-height: 24px;
    opacity: 0.8;
	}

  .app {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
    width: 100%;
		margin: 0 15px;
  }

</style>
