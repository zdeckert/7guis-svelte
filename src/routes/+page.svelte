<script lang="ts">
    let count = $state(0);
    function incramentCount() {
        count++;
    }
</script>

<h1>Svelte 7 guis</h1>
<h2>counter</h2>
<label for="count">{count}</label>
<button id="count" onclick={incramentCount}>Count</button>
