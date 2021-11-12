<script>
	import ScoreSheet from './_ScoreSheet.svelte';

	let players = 2;

	let scores = [];

	$: scores.length > players && players >= 0 ? scores = scores.slice(0, players) : undefined;
</script>

<main>
	<h1>Yahtzee</h1>
	<label>Players <input type="number" bind:value={players} /></label>
	<section>
		{#each Array(players < 0 ? 0 : players) as _, playerIndex}
			<ScoreSheet bind:scores={scores[playerIndex]} />
		{/each}
	</section>
</main>

<style>
	main, section {
		display: flex;
		align-items: center;
		gap: 1rem;
	}
	main {
		flex-direction: column;
		padding-bottom: 2rem;
	}

	section {
		max-width: 80vw;
		overflow-x: auto;
	}

	@media(max-width: 768px) {
		section {
			max-width: 100%;
			padding: 1rem;
		}
	}

	section {
		max-width: 80vw;
		overflow-x: auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
</style>
