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

	function timerReset() {
		elapsed = 0;
	}

	// CRUD
	type Person = {
		name: string;
		surname: string;
	};
	let persons: Person[] = $state([{ name: 'William', surname: 'Gibson' }]);
	const uuid = crypto.randomUUID();
	let filter: string | undefined = $state();
	let filteredPersons = $derived(
		filter
			? persons.filter(({ surname }) => surname.toLowerCase().slice(0, filter?.length) === filter)
			: persons
	);

	let selected = $state();
	let selectElement: HTMLSelectElement;
	let name: string | undefined = $state();
	let surname: string | undefined = $state();

	function crudCreate() {
		if (name && surname) {
			persons.push({
				name,
				surname
			});
			name = undefined;
			surname = undefined;
		}
	}
	function crudUpdate() {
		if (name && surname && selected) {
			persons[selectElement.selectedIndex] = {
				name,
				surname
			};
		}
	}
	function crudDelete() {
		if (selected) {
			persons.splice(selectElement.selectedIndex, 1);
		}
	}
</script>

<h1>Svelte 7 guis</h1>

<ol>
	<li><a href="#counter">counter</a></li>
	<li><a href="#temperature_converter">temperature converter</a></li>
	<li><a href="#flight_booker">flight booker</a></li>
	<li><a href="#timer">timer</a></li>
	<li><a href="#crud">crud</a></li>
</ol>

<form id="counter">
	<h2>Counter</h2>
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

<form id="flight_booker" class="flex-col fit" onsubmit={onSubmit}>
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

<form id="timer" class="flex-col fit">
	<h2>Timer</h2>
	<label for="timer_gauge">Elapsed: {elapsed.toFixed(1)}s</label>
	<progress id="timer_gauge" value={elapsed} max={duration}></progress>
	<label for="timer_slider">Duration: {duration}</label>
	<input id="timer_slider" type="range" step={0.1} max={15} bind:value={duration} />
	<button id="timer_reset" onclick={timerReset}>reset</button>
</form>

<form id="crud" class="flex-col">
	<h2>CRUD</h2>
	<div class="flex">
		<label for="crud_prefix">Filter prefix:</label>
		<input id="crud_prefix" type="text" bind:value={filter} />
	</div>
	<div class="flex">
		<select size={5} bind:this={selectElement} bind:value={selected} style="min-width: 12rem;">
			{#each filteredPersons as { name, surname }}
				<option>{surname}, {name}</option>
			{/each}
		</select>
		<div class="flex-col">
			<div class="flex" style="justify-content: space-between; width: 100%;">
				<label for="crud_name">name:</label>
				<input id="crud_name" type="text" bind:value={name} />
			</div>
			<div class="flex">
				<label for="crud_surname">surname:</label>
				<input id="crud_surname" type="text" bind:value={surname} />
			</div>
		</div>
	</div>
	<div class="flex">
		<button id="crud_create" disabled={!name || !surname} onclick={crudCreate}>create</button>
		<button id="crud_update" disabled={!selected || !name || !surname} onclick={crudUpdate}
			>update</button
		>
		<button id="crud_delete" disabled={!selected} onclick={crudDelete}>delete</button>
	</div>
</form>

<style>
	.fit {
		width: fit-content;
	}

	.flex {
		display: flex;
		gap: 0.25rem;
	}

	.flex-col {
		display: flex;
		gap: 0.25rem;
		flex-direction: column;
	}
</style>
