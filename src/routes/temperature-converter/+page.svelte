<script lang="ts">
	// temperature converter
	let celcius: number = $state(0);
	let fahrenheit: number = $state(32);
	function fahrenheitOnChange() {
		if (fahrenheit === null) return;
		celcius = Math.round((fahrenheit - 32) * (5 / 9));
	}
	function celciusOnChange() {
		if (celcius === null) return;
		fahrenheit = Math.round(celcius * (9 / 5) + 32);
	}
</script>

<form id="temperature_converter">
	<h2>Temperature Converter</h2>
	<input
		id="celcius"
		type="number"
		min="-274"
		pattern="\d+"
		bind:value={celcius}
		onchange={celciusOnChange}
		style="width: 4rem;"
	/>
	<label for="celcius">Celcius</label>
	<span> = </span>
	<input
		id="fahrenheit"
		type="number"
		min="-460"
		pattern="\d+"
		bind:value={fahrenheit}
		onchange={fahrenheitOnChange}
		style="width: 4rem;"
	/>
	<label for="fahrenheit">Fahrenheit</label>
</form>
