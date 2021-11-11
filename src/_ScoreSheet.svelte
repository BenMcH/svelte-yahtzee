<script>
	const sortFunc = (a, b) => a - b;
	const totalTopDiceRolls = (n) => Array.from({length: 6}).map((_, index) => index * n)
	const totalAllDice = [...Array.from({length: 26}).map((_, index) => index + 5), 0].sort(sortFunc);

	const scores = {
		'1': {
			description: 'Total of ones',
			validValues: totalTopDiceRolls(1)
		},
		'2': {
			description: 'Total of twos',
			validValues: totalTopDiceRolls(2)
		},
		'3': {
			description: 'Total of threes',
			validValues: totalTopDiceRolls(3)

		},
		'4': {
			description: 'Total of fours',
			validValues: totalTopDiceRolls(4)
		},
		'5': {
			description: 'Total of fives',
			validValues: totalTopDiceRolls(5)
		},
		'6': {
			description: 'Total of sixes',
			validValues: totalTopDiceRolls(6)

		},
		'Total of Top Cells': {
			disabled: true,
			description: 'Total of 1-6'
		},
		'Bonus': {
			disabled: true,
			description: '35 points if the top if >= 63'
		},
		'Total After Bonus': {
			disabled: true,
			description: 'Add Total of All Dice'
		},
		'Three of a Kind': {
			description: 'Add Total of All Dice',
			validValues: totalAllDice
		},
		'Four of a Kind': {
			description: 'Add Total of All Dice',
			validValues: totalAllDice
		},
		'Full House': {
			description: '3/2 Dice split',
			validValues: [0, 25]
		},
		'Small Straight': {
			description: '4 Numbers in a row',
			validValues: [0, 30]
		},
		'Large Straight': {
			description: '5 Numbers in a row',
			validValues: [0, 40]
		},
		'Yahtzee': {
			description: 'All 5 dice are the same number!',
			validValues: [0, 50, 150, 250, 350, 450, 550]
		},
		'Chance': {
			description: 'Add Total of All Dice',
			validValues: totalAllDice
		},
		'Bottom Total': {
			disabled: true,
		},
		'Total': {
			disabled: true,
		}
	}


	const scoreEnums = {
		TOP_TOTAL: 'Total of Top Cells',
		TOP_BONUS: 'Bonus',
		TOP_AFTER_BONUS: 'Total After Bonus',
		BOTTOM_TOTAL: 'Bottom Total',
		TOTAL: 'Total',
	}
	const sumEntries = (arr) => arr.map(([, {value}]) => value || 0).reduce((acc = 0, curr) => acc + curr);

	$: {
		let userInputFields = Object.entries(scores).filter(([,{disabled}])=> !disabled);
		let topScores = userInputFields.filter(([key]) => /^\d/.test(key))
		let bottomScorez = userInputFields.filter((obj) => !topScores.includes(obj))

		scores[scoreEnums.TOP_TOTAL].value = sumEntries(topScores);
		scores[scoreEnums.TOP_BONUS].value = scores[scoreEnums.TOP_TOTAL].value >= 63 ? 35 : 0;

		scores[scoreEnums.TOP_AFTER_BONUS].value = scores[scoreEnums.TOP_TOTAL].value + scores[scoreEnums.TOP_BONUS].value

		scores[scoreEnums.BOTTOM_TOTAL].value = sumEntries(bottomScorez);
		
		scores[scoreEnums.TOTAL].value = scores[scoreEnums.TOP_AFTER_BONUS].value + scores[scoreEnums.BOTTOM_TOTAL].value 
	}

	let name = ''
</script>

<section>
	<label>Name<input type="text" bind:value={name} /></label>
	{#each Object.entries(scores) as [key, value]}
		<label for={key}>{key} -
			{#if value.disabled}
				<span>{scores[key].value}</span>
			{:else}
				<select name={key} bind:value={scores[key].value}>
					<option value={undefined}></option>
					{#each scores[key].validValues as val}
						<option value={val}>{val}</option>
					{/each}
				</select>
			{/if}
		</label>
	{/each}
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		align-items: flex-end;
		gap: 0.5rem;
	}

	label {
		position: relative
	}
	
	label {
		padding: 0.2rem 0rem;
	}

	input, select {
		width: 150px
	}
</style>
