<script lang="ts">
	type Circle = {
		cx: number;
		cy: number;
		r: number;
	};

	let stack;
	let circles: Circle[] = $state([]);
	function canvasOnClick() {}
</script>

<div class="flex-col">
	<h2>Circle Drawer</h2>
	<div class="flex">
		<button id="undo">undo</button>
		<button id="redo">redo</button>
	</div>
	<!-- svelte-ignore a11y_click_events_have_key_events, a11y_no_static_element_interactions -->
	<svg
		viewBox="0 0 300 200"
		width="300"
		height="200"
		xmlns="http://www.w3.org/2000/svg"
		onclick={canvasOnClick}
	>
		<circle cx="50" cy="50" r="50" stroke="black" stroke-width="2" fill="none" />
	</svg>
</div>

<style>
	svg {
		border: 1px solid black;
	}
</style>
