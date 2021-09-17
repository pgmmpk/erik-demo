<script lang="ts">
	export let name;

	import Button from '@smui/button/Button.svelte';
	import Paper from '@smui/paper';

	async function sleep(time) {
		return new Promise(resolve => setTimeout(resolve, time));
	}

	let session = [];

	let value = '';
	function clicked() {
		const item = {
			prompt: value,
			prediction: ''
		};

		const history = session.map(x => x.prediction);
		runPrediction(value, history, item);

		session.push(item);
		value = '';
		session = session;
	}

	var loading = false;
	async function runPrediction(prompt, history, item) {
		loading = true;
		item.prediction = 'Please wait...';
		session = session;

		await sleep(3000);
		item.prediction = 'for i in range(20):\n\tprint(i)'
		loading = false;
		session = session;
	}
</script>

<main>
	<Paper />
	{#each session as item}
	<Paper>{item.prompt}</Paper>
	<Paper><pre>{item.prediction}</pre></Paper>
	{/each}
	<input type='text' bind:value={value} >
	<Button variant="raised" on:click={clicked} disabled={loading}>Go</Button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>