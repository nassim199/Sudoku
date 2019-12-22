<script>
  import SudokuGrid from "./components/SudokuGrid.svelte";
  import Numpad from "./components/Numpad.svelte";
  
  import { game, solution } from "./Sudoku games/sudoku_games.js";
  export let name;

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

  let gridEvaluation = [
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true],
      [true, true, true, true, true, true, true, true, true]
    ];

  document.addEventListener("keyup", event => {
    if (event.keyCode >= 48 && event.keyCode <= 57) {
      grid[selectedCell.i][selectedCell.j] = event.keyCode - 48;
        gridEvaluation[selectedCell.i][selectedCell.j] = 
        grid[selectedCell.i][selectedCell.j] === solution[selectedCell.i][selectedCell.j];
    } else if (event.keyCode === 32) {
      grid[selectedCell.i][selectedCell.j] = 0;
      gridEvaluation[selectedCell.i][selectedCell.j] = true;
    } else {
      return;
    }

    grid = grid;
  });
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 2em;
    font-weight: 100;
  }

  div {
    /* margin: auto; */
    text-align: center;
  }
</style>

<main>
  <h1>{message}!</h1>
  <div>
    <SudokuGrid {grid} {selectedCell} gridEvaluation={gridEvaluation} on:select={selectCell} />
  </div>
    <Numpad />
</main>
