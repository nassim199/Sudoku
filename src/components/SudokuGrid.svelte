<script>
  import Cell from "./Cell.svelte";
  export let grid;
  export let selectedCell;
  export let gridEvaluation;

  const sameCube = (firstCell, secondCell) => {
      let i1 = ( firstCell.i - (firstCell.i % 3) ) / 3;
      let i2 = ( secondCell.i - (secondCell.i % 3) ) / 3;
      let j1 = ( firstCell.j - (firstCell.j % 3) ) / 3;
      let j2 = ( secondCell.j - (secondCell.j % 3) ) / 3;

      return (i1 === i2 && j1 === j2);
  };

</script>

<style>
  tr,
  td {
    margin: 0;
    padding: 0;
  }
</style>

<table CELLSPACING="0">
  {#each grid as gridColumn, i}
    <tr>
      {#each gridColumn as gridCell, j}
        <td>
          <Cell
            value={gridCell}
            {i}
            {j}
            selected={i === selectedCell.i && j === selectedCell.j}
            pseudoSelected={i === selectedCell.i || j === selectedCell.j || sameCube({i,j}, selectedCell)}
            valid={gridEvaluation[i][j]}
            on:select/>
        </td>
      {/each}
    </tr>
  {/each}
</table>
