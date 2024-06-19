<script>
  import { onMount } from "svelte";

  const symbols = ["♦️", "♣️", "♥️", "♠️"];
  const values = [11, 2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10];
  const faces = ["A", ...values.slice(1, -3), "J", "Q", "K"];
  const allCards = symbols.flatMap((symbol, idx) =>
    faces.map((face, index) => ({ value: values[index], face: face, symbol: symbol, red: idx % 2 == 0 }))
  );

  let cards = $state([]);
  let opponents = $state([[], []]);

  function restart() {
    cards = allCards.toSorted(() => Math.random() - 0.5);
    opponents.forEach((opponent, index) => {
      opponents[index] = [cards.pop()];
      opponents[index].push(cards.pop());
    });
  }

  onMount(() => {
    restart();
  });
</script>

<div class="flex flex-wrap justify-center gap-4 text-center">
  <div class="">
    <button
      class="text-blue-500"
      onclick={() => {
        restart();
      }}>[o]</button
    >
  </div>
  {#each opponents as opponent, index}
    <div class="flex flex-col gap-2 border border-gray-500 px-2">
      <div class="">
        <span class="">{index + 1}</span>
        {#if index > 0}
          <button
            class="text-red-500"
            onclick={() => {
              opponents.splice(index, 1);
            }}>[x]</button
          >
        {/if}
      </div>
      {#each opponent as card}
        <div class="" class:text-red-500={card.red}>
          <span class="font-bold text-2xl">{card.face}</span>
          <span class="text-4xl">{card.symbol}</span>
        </div>
      {/each}
    </div>
  {/each}
  {#if opponents.length < 25}
    <div class="">
      <button
        class="text-blue-500"
        onclick={() => {
          opponents.push([]);
          restart();
        }}>[+]</button
      >
    </div>
  {/if}
</div>
