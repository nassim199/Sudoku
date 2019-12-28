<script>
  import SudokuGrid from "./components/SudokuGrid.svelte";
  import Numpad from "./components/Numpad.svelte";
  import Timer from "./components/Timer.svelte";

  import {
    game,
    solution,
    startGridEvaluation,
    immutable
  } from "./Sudoku games/sudoku_games.js";

  let message = "Hello";
  let gridEvaluation = startGridEvaluation;
  let pauseGrid = Array(9).fill(Array(9).fill(0));

  let start = false;
  let stop = false;

  let grid = game;
  let validCells = immutable.reduce((total, arr) => {
    return (
      total +
      arr.reduce((total, val) => {
        return total + (val ? 1 : 0);
      }, 0)
    );
  }, 0);

  let selectedCell = {
    i: 9,
    j: 9
  };
  function selectCell(event) {
    if (start) {
      selectedCell = {
        i: event.detail.i,
        j: event.detail.j
      };
    }
  }

  let time = {
    s: 0,
    m: 0,
    h: 0
  };

  function selectNumber(number) {
    if (stop || !start) return;
    if (selectedCell.i === 9 || selectedCell.j === 9) return;

    if (number >= 48 && number <= 57) {
      if (immutable[selectedCell.i][selectedCell.j]) return;
      if (
        gridEvaluation[selectedCell.i][selectedCell.j] &&
        grid[selectedCell.i][selectedCell.j] !== 0
      )
        validCells--;
      grid[selectedCell.i][selectedCell.j] = number - 48;
      gridEvaluation[selectedCell.i][selectedCell.j] =
        grid[selectedCell.i][selectedCell.j] ===
        solution[selectedCell.i][selectedCell.j];
      if (gridEvaluation[selectedCell.i][selectedCell.j]) validCells++;
    } else if (number === 32) {
      if (immutable[selectedCell.i][selectedCell.j]) return;
      if (
        gridEvaluation[selectedCell.i][selectedCell.j] &&
        grid[selectedCell.i][selectedCell.j] !== 0
      )
        validCells--;
      grid[selectedCell.i][selectedCell.j] = 0;
      gridEvaluation[selectedCell.i][selectedCell.j] = true;
    } else if (number === 38) {
      if (selectedCell.i !== 0) selectedCell.i--;
    } else if (number === 37) {
      if (selectedCell.j !== 0) selectedCell.j--;
    } else if (number === 39) {
      if (selectedCell.j !== 8) selectedCell.j++;
    } else if (number === 40) {
      if (selectedCell.i !== 8) selectedCell.i++;
    } else {
      return;
    }

    grid = grid;
    selectedCell = selectedCell;
    console.log(validCells);
    if (validCells === 81) {
      message = "CONGRATULATIONS";
      stop = true;
    }
  }

  document.addEventListener("keydown", event => {
    selectNumber(event.keyCode);
  });
  document.addEventListener("click", _ => {
    selectedCell = {
      i: 9,
      j: 9
    };
  });
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
    background-color: rgba(206, 238, 253, 0.555);
  }

  .win-grid {
    background-color: rgba(224, 253, 226, 0.589);
  }
</style>

<main>
  <h1>{message}!</h1>
  <div class="{stop ? 'blurred-grid' : ''}
              {validCells === 81 ? 'win-grid' : ''}">
    {#if start || stop}
      <SudokuGrid
        grid={stop ? pauseGrid : grid}
        {selectedCell}
        {gridEvaluation}
        {immutable}
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
      <Timer startTimer={start} {time} on:time={e => (time = e.detail.time)} />
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
        <button
          on:click={() => {
            stop = false;
            start = true;
          }}>
          Resume
        </button>
      {/if}
    {/if}
  </div>
</main>
