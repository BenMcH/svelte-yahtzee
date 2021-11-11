<script>
	import ScoreSheet from './_ScoreSheet.svelte';

	let players = 2;
</script>

<main>
	<h1>Yahtzee</h1>
	<label>Players <input type="number" bind:value={players} /></label>
	<section>
		{#each Array(players < 0 ? 0 : players) as _, playerIndex}
			<ScoreSheet />
		{/each}
	</section>
</main>

<style>
	section {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 1rem;
	}
	main {
		text-align: center;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
</style>
