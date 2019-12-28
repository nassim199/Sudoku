<script>
  import { createEventDispatcher } from "svelte";

  const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

  const dispatch = createEventDispatcher();

  function selectNumber(i) {
    let j = i + 48;
    dispatch("selectNumber", {
      j
    });
  }

</script>

<style>
ul {
  display: table;
  border-spacing: 0.5rem;
}
li {
  display: table-cell;
  height: 2rem;
  width: 2rem;
  border: 1px solid rgba(0, 0, 0, 0.4);
  border-radius: 3px / 2px;
  text-align: center;
  vertical-align: middle;
  font-size: 1.2rem;
}

li:hover {
  border-color: #fff;
  color: #fff;
  background-color: rgba(22, 22, 22, 0.8);
}
</style>

<ul>
    {#each numbers as number}
      <li on:click={e => selectNumber(number)}>{number}</li>
    {/each}
    <li on:click|stopPropagation={e => selectNumber(0)}>x</li>
</ul>

