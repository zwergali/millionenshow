<script lang="ts">
	import { fragen } from '../lib/Fragen/fragen';

	let ausgewählteFrage = $state(1);

	const frage = $derived(fragen[ausgewählteFrage]);

	let falschGewählteAntworten: number[] = $state([]);

	function onButtonClick(index: number) {
		const gewählteAntwort = frage.antworten[index];
		if (gewählteAntwort === frage.richtige_antwort) {
			ausgewählteFrage++;
			falschGewählteAntworten = [];
		} else {
			falschGewählteAntworten.push(index);
		}
	}
</script>

<main>
	<h1>{frage.frage}</h1>
	<div class="buttons">
		{#each [0, 1, 2, 3] as index}
			<button
				onclick={() => onButtonClick(index)}
				class={falschGewählteAntworten.includes(index) ? 'ungültig' : ''}
			>
				{frage.antworten[index]}
			</button>
		{/each}
	</div>
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		width: 100%;
		min-height: 100%;
		padding: 20px;
		gap: 40px;
	}

	h1 {
		flex-grow: 1;
		text-align: center;
		display: flex;
		justify-content: center;
		align-items: flex-end;
	}

	.buttons {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 40px;
	}
	button {
		min-height: 60px;
	}

	.ungültig {
		background-color: red;
	}
</style>
