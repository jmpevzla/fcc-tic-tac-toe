<script>
  import Cell from "./Cell.svelte";
  import { createEventDispatcher } from "svelte";

  export let selectedPlayers;
  export let player1;

  const dispatch = createEventDispatcher();

  const player2 = player1 === "X" ? "O" : "X";

  const phases = Object.freeze({
    play1: "play1",
    play2: "play2",
    cpu: "cpu",
    end: "end",
  });

  const results = Object.freeze({
    WinPlayer1: "WinPlayer1",
    WinPlayer2: "WinPlayer2",
    WinCPU: "WinCPU",
    Draw: "Draw",
  });

  const diffHard = Math.floor(Math.random() * 2);
  
  let phase = phases.play1;
  let result = results.Draw;
  let cells = [];
  let matrix = [];

  $: getHeader = () => {
    switch (phase) {
      case "play1":
      case "play2":
        return `Player ${phase.slice(phase.length - 1)}`;
      case "cpu":
        return "CPU";
      case "end":
        const map = {
          [results.WinPlayer1]: "Player 1 Won!",
          [results.WinPlayer2]: "Player 2 Won!",
          [results.WinCPU]: "You Lost!",
          [results.Draw]: "Draw!",
        };
        return map[result];
    }
  };

  let i = -1;
  for (let j = 0; j < 9; j++) {
    const jmod = j % 3;
    if (jmod === 0) {
      matrix.push([]);
      i++;
    }
    cells.push([i, jmod]);
    matrix[i][jmod] = "";
  }

  function changeCell(x, y, value) {
    matrix[x][y] = value;
  }

  function endGame(winner = "") {
    phase = phases.end;

    if (player1 === winner) {
      result = results.WinPlayer1;
    } else if (player2 === winner && selectedPlayers === 1) {
      result = results.WinCPU;
    } else if (player2 === winner) {
      result = results.WinPlayer2;
    } else {
      result = results.Draw;
    }
  }

  function checkGame() {
    let emptyCells = false;
    for (let i = 0; i < 3; i++) {
      for (let j = 0; j < 3; j++) {
        if (matrix[i][j] === "") {
          emptyCells = true;
          break;
        }
      }
    }

    let value = matrix[0][0];
    if (
      value !== "" &&
      ((value === matrix[0][1] && value === matrix[0][2]) ||
        (value === matrix[1][0] && value === matrix[2][0]) ||
        (value === matrix[1][1] && value === matrix[2][2]))
    ) {
      return endGame(value);
    }
    value = matrix[0][1];
    if (value !== "" && value === matrix[1][1] && value === matrix[2][1]) {
      return endGame(value);
    }
    value = matrix[0][2];
    if (
      value !== "" &&
      ((value === matrix[1][1] && value === matrix[2][0]) ||
        (value === matrix[1][2] && value === matrix[2][2]))
    ) {
      return endGame(value);
    }
    value = matrix[1][0];
    if (value !== "" && value === matrix[1][1] && value === matrix[1][2]) {
      return endGame(value);
    }
    value = matrix[2][0];
    if (value !== "" && value === matrix[2][1] && value === matrix[2][2]) {
      return endGame(value);
    }

    if (!emptyCells) {
      endGame();
    }
  }

  function getCpuIdx() {
 
    let activeHint = false;
    if (diffHard) {
      activeHint = true;
    } else {
      activeHint = Math.floor(Math.random() * 2);
    }

    if (activeHint) {
      if (matrix[0][0] === player1) {
        if (matrix[0][1] === player1 && matrix[0][2] === '') {
          return [0, 2];
        }
        if (matrix[1][0] === player1 && matrix[2][0] === '') {
          return [2, 0];
        }
        if (matrix[1][1] === player1 && matrix[2][2] === '') {
          return [2, 2];
        }
      }

      if (matrix[0][1] === player1) {
        if (matrix[0][2] === player1 && matrix[0][0] === '') {
          return [0, 0];
        }
        if (matrix[1][1] === player1 && matrix[2][1] === '') {
          return [2, 1];
        }
      }

      if (matrix[0][2] === player1) {
        if (matrix[1][1] === player1 && matrix[2][0] === '') {
          return [2, 0];
        }
        if (matrix[1][2] === player1 && matrix[2][2] === '') {
          return [2, 2];
        }
      }

      if (matrix[1][0] === player1) {
        if (matrix[1][1] === player1 && matrix[1][2] === '') {
          return [1, 2];
        }
      }

      if (matrix[1][1] === player1) {
        if (matrix[2][1] === player1 && matrix[0][1] === '') {
          return [0, 1];
        }
        if (matrix[2][2] === player1 && matrix[0][0] === '') {
          return [0, 0];
        }
        if (matrix[2][0] === player1 && matrix[0][2] === '' ) {
          return [0, 2];
        }
        if (matrix[1][2] === player1 && matrix[1, 0] === '') {
          return [1, 0];
        }
      }

      if (matrix[1][2] === player1) {
        if (matrix[2][2] === player1 && matrix[0][2] === '') {
          return [0, 2];
        }
      }

      if (matrix[2][0] === player1) {
        if (matrix[0][0] === player1 && matrix[1][0] === '') {
          return [1, 0];
        }
        if (matrix[2][2] === player1 && matrix[2][1] === '') {
          return [2, 1];
        }
        if (matrix[0][2] === player1 && matrix[1][1] === '') {
          return [1, 1];
        }
        if (matrix[2][1] === player1 && matrix[2][2] === '') {
          return [2, 2];
        }
      }

      if (matrix[2][1] === player1) {
        if (matrix[2][2] === player1 && matrix[2][0] === '') {
          return [2, 0];
        }
        if (matrix[0][1] === player1 && matrix[1][1] === '') {
          return [1, 1];
        }
      }

      if (matrix[2][2] === player1) {
        if (matrix[0][0] === player1 && matrix[1][1] === '') {
          return [1, 1];
        }
        if (matrix[0][2] === player1 && matrix[1][2] === '') {
          return [1, 2];
        }
      }
    }

    const cpuxy = [];
        
    for (let i = 0; i < 3; i++) {
      for (let j = 0; j < 3; j++) {
        if (matrix[i][j] === "") {
          cpuxy.push([i, j]);
        }
      }
    }

    return cpuxy[Math.floor(Math.random() * cpuxy.length)];
  }

  function engine(x, y) {
    if (phase === phases.end || phase === phases.cpu) {
      return;
    }

    if (phase === phases.play1) {
      changeCell(x, y, player1);

      checkGame();
      if (phase !== phases.end) {
        if (selectedPlayers === 2) {
          phase = phases.play2;
        } else {
          phase = phases.cpu;

          const cpuIdx = getCpuIdx(); 
          setTimeout(() => {
            const [cpux, cpuy] = cpuIdx;
            changeCell(cpux, cpuy, player2);
            phase = phases.play1;
            return checkGame();
          }, 1000);
        }
      }
    } else if (phase === phases.play2) {
      changeCell(x, y, player2);
      phase = phases.play1;
      return checkGame();
    }
  }

  function onCellTouch({ detail: { idx } }) {
    const x = idx[0];
    const y = idx[1];

    if (matrix[x][y] === "") {
      engine(x, y);
    }
  }

  function onResetClick() {
    dispatch("reset");
  }
</script>

<div class="game">
  <p>Begin Game! {selectedPlayers}-{player1}</p>

  <header>
    <h2>
      {#if phase === phases.end}
        <span>The game is over!</span>
      {:else}
        <span>Turn: </span>
      {/if}
      {getHeader()}
    </h2>
    <button on:click={onResetClick}>Reset Game!</button>
  </header>

  <div class="board">
    {#each cells as cell, i (i)}
      <Cell
        idx={cell}
        value={matrix[cell[0]][cell[1]]}
        on:cellTouch={onCellTouch}
      />
    {/each}
  </div>
</div>

<style>
  .board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 5px;
  }
</style>
