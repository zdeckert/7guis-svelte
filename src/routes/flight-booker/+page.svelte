<script lang="ts">
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
</script>

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
