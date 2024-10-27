<script lang="ts">
	// Timer
	let duration = $state(10);
	let elapsed = $state(0);
	function reset() {
		elapsed = 0;
	}
	let intervalId;
	intervalId = setInterval(() => {
		if (elapsed < duration) elapsed += 0.1;
	}, 100);

	function timerReset() {
		elapsed = 0;
	}
</script>

<form id="timer" class="flex-col fit">
	<h2>Timer</h2>
	<label for="timer_gauge">Elapsed: {elapsed.toFixed(1)}s</label>
	<progress id="timer_gauge" value={elapsed} max={duration}></progress>
	<label for="timer_slider">Duration: {duration}</label>
	<input id="timer_slider" type="range" step={0.1} max={15} bind:value={duration} />
	<button id="timer_reset" onclick={timerReset}>reset</button>
</form>
