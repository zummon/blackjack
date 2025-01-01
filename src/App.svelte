<script>
	import { onMount } from 'svelte'
	
	const symbols = ["♦️", "♣️", "♥️", "♠️"];
	const values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10];
	const faces = ["A", ...values.slice(1, -3), "J", "Q", "K"];
	const allCards = symbols.flatMap((symbol, idx) =>
		faces.map((face, index) => ({
			value: values[index],
			face: face,
			symbol: symbol,
			red: idx % 2 == 0
		}))
	);

	let cards = $state([])
	let opponents = $state([[], []])

	function restart() {
		cards = allCards.toSorted(() => Math.random() - 0.5);
		opponents.forEach((opponent, index) => {
			opponents[index] = [cards.pop(), cards.pop()];
		});
	}

	onMount(() => {
		restart();
	})

</script>


<div class="flex flex-wrap justify-center items-center gap-4 text-center py-2">
	{#each opponents as opponent, index}
		<div class="flex flex-col gap-2 px-2">
			<div class="">
				<span class="leading-8 font-semibold">Player <span>{index + 1}</span></span>
				{#if index > 1}
					<button class="text-red-500 text-2xl" onclick={() => {
						opponents[index].forEach((card) => {
							cards.push(card);
						});
						opponents.splice(index, 1);
					}}>✘</button>
				{/if}
			</div>
			{#each opponent as card}
				<div class="bg-white rounded pl-2 pr-2 pb-2 whitespace-nowrap {card.red ? 'text-red-500' : 'text-black'}">
					<span class="font-bold text-2xl">{card.face}</span>
					<span class="text-4xl align-top">{card.symbol}</span>
				</div>
			{/each}
			<div class="">
				<span class="font-semibold">
					(<span>{
						opponent.reduce((prev, curr) => {
							return prev + curr.value;
						}, 0)
					}</span>)
				</span>
				<button class="text-2xl align-top" onclick={() => {
					opponents[index].push(cards.pop());
				}}>✚</button>
			</div>
		</div>
	{/each}
	{#if opponents.length < 25}
		<div class="">
			<div class="">
				<button class="text-4xl" onclick={() => {
					restart();
				}}>↻</button>
			</div>
			<button class="text-4xl" onclick={() => {
				opponents.push([cards.pop(), cards.pop()]);
			}}>✚</button>
		</div>
	{/if}
</div>

