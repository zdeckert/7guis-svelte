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

	// Flight Booker
	let flightType = $state('one_way');
	let departureDate = $state(new Date().toISOString().slice(0, 10));
	let returnDate = $state(new Date().toISOString().slice(0, 10));
	function onSubmit() {
		let msg =
			flightType === 'one_way'
				? `You booked a one-way flight on ${departureDate}`
				: `You booked a round trip flight departing on ${departureDate} and returning ${departureDate}`;
		alert(msg);
	}

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

	// function timerStart() {
	// 	if (!intervalId) intervalId = setInterval(() => (elapsed += 0.1), 100);
	// }

	function timerReset() {
		elapsed = 0;
		// clearInterval(intervalId!);
		// intervalId = null;
	}
</script>

<h1>Svelte 7 guis</h1>

<form id="counter">
	<h2>counter</h2>
	<label for="count">{count}</label>
	<button id="count" onclick={incramentCount}>Count</button>
</form>

<form id="temperature_converter">
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
</form>

<form id="flight_booker" onsubmit={onSubmit}>
	<h2>Flight booker</h2>
	<select bind:value={flightType} id="flight_select">
		<option value="one_way">one way flight</option>
		<option value="return">return flight</option>
	</select>

	<!-- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/date#additional_attributes
	using new Date().toISOString().slice(0, 10) to match 'min' atrribute format
	of a yyyy-mm-dd string.  -->
	<input
		id="departure_date"
		type="date"
		min={new Date().toISOString().slice(0, 10)}
		bind:value={departureDate}
	/>
	<input
		id="return_date"
		type="date"
		min={departureDate}
		disabled={flightType === 'one_way'}
		bind:value={returnDate}
	/>
	<button type="submit">Book</button>
</form>

<form id="timer">
	<h2>Timer</h2>
	<label for="timer_gauge">Elapsed: {elapsed.toFixed(1)}s</label>
	<progress id="timer_gauge" value={elapsed} max={duration}></progress>
	<label for="timer_slider">Duration: {duration}</label>
	<input id="timer_slider" type="range" step={0.1} max={15} bind:value={duration} />
	<button id="timer_reset" onclick={timerReset}>reset</button>
</form>

<style>
	#flight_booker,
	#timer {
		display: flex;
		flex-direction: column;
		width: fit-content;
		gap: 0.25rem;
	}
</style>
