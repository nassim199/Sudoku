<script>
  import SudokuGrid from "./components/SudokuGrid.svelte";
  export let name;
  let grid = [
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
    [0, 2, 3, 4, 5, 6, 7, 8, 9],
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
    [1, 2, 3, 4, 5, 6, 7, 8, 9]
  ];
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

  document.addEventListener("keyup", event => {
	  
	  if (event.keyCode >= 48 && event.keyCode <= 57) {
		  grid[selectedCell.i][selectedCell.j] = event.keyCode - 48;
	  } else if (event.keyCode === 32) {
		  grid[selectedCell.i][selectedCell.j] = 0;
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
    font-size: 4em;
    font-weight: 100;
  }

  div {
    /* margin: auto; */
    text-align: center;
  }
</style>

<main>
  <h1>Hello {name}!</h1>
  <div>
    <SudokuGrid {grid} {selectedCell} on:select={selectCell} />
  </div>
</main>
