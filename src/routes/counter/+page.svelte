<script lang="ts">
	let count = $state(0);
	function incramentCount() {
		count++;
	}
</script>

<form id="counter">
	<h2>Counter</h2>
	<label for="count">{count}</label>
	<button id="count" onclick={incramentCount}>Count</button>
</form>
