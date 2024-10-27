<script lang="ts">
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
