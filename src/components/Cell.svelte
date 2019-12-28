<script>
  import { createEventDispatcher } from "svelte";

  export let value;
  export let i;
  export let j;
  export let selected = false;
  export let pseudoSelected = false;
  export let valid;
  export let immutable = false;

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
    border: 0.5px solid rgba(0, 0, 0, 0.3);
    /*TODO: 1px border for mobiles*/
    border-radius: 1px / 1px;
    color: rgb(0, 81, 173);
    font-weight: 500;
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
  .immutable {
    color: rgb(59, 59, 59);
  }
  .pseudo-selected {
    background-color: rgba(85, 128, 141, 0.24);
  }
  .selected {
    background-color: rgba(122, 189, 252, 0.699);
  }
  .wrong {
    color: rgb(46, 17, 12);
    background-color: rgba(209, 155, 155, 0.39);
  }
</style>

<div
  class="{topBorder ? 'top-border' : ''}
  {bottomBorder ? 'bottom-border' : ''}
  {leftBorder ? 'left-border' : ''}
  {rightBorder ? 'right-border' : ''}
  {pseudoSelected ? 'pseudo-selected' : ''}
  {selected ? 'selected' : ''}
  {immutable ? 'immutable' : ''}
  {valid ? '' : 'wrong'}"
  on:click|stopPropagation={selectCell}>
  {value || ''}
</div>
