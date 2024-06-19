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

<div class="flex flex-wrap justify-center items-baseline gap-4 text-center">
  <div class="">
    <button
      class="text-blue-300 text-4xl"
      onclick={() => {
        restart();
      }}>↻</button
    >
  </div>
  {#each opponents as opponent, index}
    <div class="flex flex-col gap-2 px-2">
      <div class="">
        <span class="">Player {index + 1}</span>
        {#if index > 1}
          <button
            class="text-red-500 text-2xl"
            onclick={() => {
              opponents.forEach((opponent) => {
                opponent.forEach((card) => {
                  cards.push(card);
                });
              });

              opponents.splice(index, 1);
            }}>✘</button
          >
        {/if}
      </div>
      {#each opponent as card}
        <div
          class="bg-white flex gap-1 rounded pl-2 pr-2 pb-2"
          class:text-red-500={card.red}
          class:text-black={!card.red}
        >
          <div class="font-bold text-2xl">{card.face}</div>
          <div class="text-4xl">{card.symbol}</div>
        </div>
      {/each}
    </div>
  {/each}
  {#if opponents.length < 25}
    <div class="">
      <button
        class="text-blue-300 text-4xl"
        onclick={() => {
          opponents.push([]);
          opponents[opponents.length - 1] = [cards.pop()];
          opponents[opponents.length - 1].push(cards.pop());
        }}>✚</button
      >
    </div>
  {/if}
</div>
