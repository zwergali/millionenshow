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
	<div class="box">
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
	</div>
	<div class="bg"></div>
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		width: 100%;
		min-height: 100%;
		padding: 20px;
		position: relative;
		justify-content: flex-end;
	}

	.box {
		display: flex;
		flex-direction: column;
		gap: 40px;
		background-color: #a3a3a391;
		border-radius: 16px;
		padding: 20px;
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

	.bg {
		overflow: hidden;
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		left: 0;
		z-index: -1;

		container-type: size;
	}

	.bg::after {
		content: '';
		display: block;
		height: 200cqmax;
		width: 200cqmax;
		position: absolute;
		top: 50%;
		left: 50%;
		translate: -50% -50%;
		background-size: 100% 100%;
		background-position: 0px 0px;
		animation: rotation 40s linear 0s infinite forwards;

		--c1: hsl(117 58% 38%);
		--c2: hsl(233 100% 42%);
		background-image:
			radial-gradient(#c7cad6, #c7cad6 20px, #c7cad600 100px),
			conic-gradient(
				var(--c1),
				var(--c2),
				var(--c1),
				var(--c2),
				var(--c1),
				var(--c2),
				var(--c1),
				var(--c2),
				var(--c1)
			);
	}

	@keyframes rotation {
		0% {
			rotate: 0deg;
		}
		100% {
			rotate: 360deg;
		}
	}
</style>
