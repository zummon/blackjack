<script>
let symbols = ['♠️','♥️','♦️','♣️']
let values = [1,2,3,4,5,6,7,8,9,10,10,10,10]
let faces = ['A',...values.slice(1,-3),'J','Q','K']
let cards = symbols.flatMap((symbol) => faces.map((face) => face + symbol))
</script>

<div class="flex flex-wrap justify-center gap-4 text-center">
{#each cards as card}
<div class="">
{card}
</div>
{/each}
</div>