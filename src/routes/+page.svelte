<script lang="ts">
	// counter
	let count = $state(0);
	function incramentCount() {
		count++;
	}

	// temperature converter
	let celcius: number = $state(0);
	function celciusOnChange() {
		if (celcius === null) return;
		fahrenheit = Math.round(celcius * (9 / 5) + 32);
	}
	let fahrenheit: number = $state(32);
	function fahrenheitOnChange() {
		if (fahrenheit === null) return;
		celcius = Math.round((fahrenheit - 32) * (5 / 9));
	}
	$inspect(celcius, fahrenheit);
</script>

<h1>Svelte 7 guis</h1>

<h2>counter</h2>
<label for="count">{count}</label>
<button id="count" onclick={incramentCount}>Count</button>

<form>
	<h2>Temperature Converter</h2>
	<input
		id="celcius"
		type="number"
		min="-274"
		pattern="\d+"
		bind:value={celcius}
		onchange={celciusOnChange}
	/>
	<label for="celcius">Celcius</label><span> = </span>
	<label for="fahrenheit">Fahrenheit</label>
	<input
		id="fahrenheit"
		type="number"
		min="-460"
		pattern="\d+"
		bind:value={fahrenheit}
		onchange={fahrenheitOnChange}
	/>
	C:{celcius} F:{fahrenheit}
</form>
