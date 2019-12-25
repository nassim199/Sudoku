<script>
  import SudokuGrid from "./components/SudokuGrid.svelte";
  import Numpad from "./components/Numpad.svelte";
  import Timer from "./components/Timer.svelte";

  import { game, solution } from "./Sudoku games/sudoku_games.js";

  let message = "Hello";
  let grid = game;
  let selectedCell = {
    i: 0,
    j: 0
  };
  function selectCell(event) {
    selectedCell = {
      i: event.detail.i,
      j: event.detail.j
    };
  }

  let time = {
    s: 0,
    m: 0,
    h: 0
  };

  let gridEvaluation = Array(9).fill(Array(9).fill(true));

  function selectNumber(i) {
    if (stop) 
      return;

    if (i >= 48 && i <= 57) {
      grid[selectedCell.i][selectedCell.j] = i - 48;
      gridEvaluation[selectedCell.i][selectedCell.j] =
        grid[selectedCell.i][selectedCell.j] ===
        solution[selectedCell.i][selectedCell.j];
    } else if (i === 32) {
      grid[selectedCell.i][selectedCell.j] = 0;
      gridEvaluation[selectedCell.i][selectedCell.j] = true;
    } else if (i === 38) {
      if (selectedCell.i !== 0) selectedCell.i--;
    } else if (i === 37) {
      if (selectedCell.j !== 0) selectedCell.j--;
    } else if (i === 39) {
      if (selectedCell.j !== 8) selectedCell.j++;
    } else if (i === 40) {
      if (selectedCell.i !== 8) selectedCell.i++;
    } else {
      return;
    }

    grid = grid;
    selectedCell = selectedCell;
  }

  document.addEventListener("keydown", event => {
    selectNumber(event.keyCode);
  });

  let start = false;
  let stop = false;
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    display: flex;
    flex-direction: column;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 2em;
    font-weight: 100;
  }

  div {
    display: table-cell;
    margin: 0 auto;
    /* text-align: center; */
  }

  .blurred-grid {
    background-color: rgba(185, 228, 248, 0.555);
  }
</style>

<main>
  <h1>{message}!</h1>
  <div class={stop ? 'blurred-grid' : ''}>
    {#if start || stop}
      <SudokuGrid
        {grid}
        {selectedCell}
        {gridEvaluation}
        on:select={selectCell} />
    {:else}
      <button on:click={() => (start = true)}>Start Playing</button>
    {/if}
  </div>
  <div>
    <Numpad on:selectNumber={e => selectNumber(e.detail.j)} />
  </div>
  <div id="timer">
    {#if start}
      <Timer startTimer={start} {time} on:time={(e) => time = e.detail.time}/>
      <button
        on:click={() => {
          start = false;
          stop = true;
        }}>
        Pause
      </button>
    {:else}
      <div>{time.h} : {time.m} : {time.s}</div>
      {#if stop}
        <button on:click={() => {stop = false; start = true}} > Resume </button> 
      {/if}
    {/if}
  </div>
</main>
