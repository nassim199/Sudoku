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

  // let gridEvaluation = [
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true],
  //     [true, true, true, true, true, true, true, true, true]
  //   ];
  let gridEvaluation = Array(9).fill(Array(9).fill(true));

  function selectNumber(i) {

    if (i >= 48 && i <= 57) {
      grid[selectedCell.i][selectedCell.j] = i - 48;
        gridEvaluation[selectedCell.i][selectedCell.j] = 
        grid[selectedCell.i][selectedCell.j] === solution[selectedCell.i][selectedCell.j];
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

  #grid::after {
    width: 50vw;
    height: 50vh;
    background-color: antiquewhite;
    content: 'Continue';
  }
</style>

<main>
  <h1>{message}!</h1>
  <div id="grid">
  {#if start}
    <SudokuGrid {grid} {selectedCell} gridEvaluation={gridEvaluation} on:select={selectCell} />
  {:else} 
    <button on:click={() => start = true}>Start Playing</button>
  {/if}
  </div>
  <div>
    <Numpad on:selectNumber={e => selectNumber(e.detail.j)}/>
  </div>
  <div id="timer">
    <Timer bind:startTimer={start}/>
    {#if start}
    <button on:click={() => start = false}> Pause </button>
    {/if}
  </div>
</main>
