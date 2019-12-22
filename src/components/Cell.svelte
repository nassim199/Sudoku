<script>
  import { createEventDispatcher } from "svelte";

  export let value;
  export let i;
  export let j;
  export let selected = false;
  export let pseudoSelected = false;
  export let valid = true;

  const topBorder = i % 3 === 0;
  const bottomBorder = i === 8;
  const leftBorder = j % 3 === 0;
  const rightBorder = j === 8;

  const dispatch = createEventDispatcher();

  function selectCell() {
    dispatch("select", {
      i,
      j
    });
  }
</script>

<style>
  div {
    height: 2rem;
    width: 2rem;
    border: 1px solid rgba(0, 0, 0, 0.5);
    border-radius: 1px / 1px;
    -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
    -moz-user-select: none; /* Old versions of Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
    user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Opera and Firefox */
  }

  .top-border {
    border-top: 1.4px solid rgba(0, 0, 0, 0.7);
  }
  .bottom-border {
    border-bottom: 1.4px solid rgba(0, 0, 0, 0.7);
  }
  .left-border {
    border-left: 1.4px solid rgba(0, 0, 0, 0.7);
  }
  .right-border {
    border-right: 1.4px solid rgba(0, 0, 0, 0.7);
  }
  .pseudo-selected {
    background-color: rgba(95, 158, 160, 0.596);
  }
  .selected {
    background-color: cadetblue;
  }
  .wrong {
    color: rgb(90, 2, 2);
    background-color: rgba(160, 95, 95, 0.493);
  }
</style>

<div
  class="{topBorder ? 'top-border' : ''}
  {bottomBorder ? 'bottom-border' : ''}
  {leftBorder ? 'left-border' : ''}
  {rightBorder ? 'right-border' : ''}
  {pseudoSelected ? 'pseudo-selected' : ''}
  {selected ? 'selected' : ''}
  {valid ? '' : 'wrong'}"
  on:click={selectCell}>
  {value || ''}
</div>
