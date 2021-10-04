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
  let showBegin = true;
  let winCells = [];

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

  function init() {
    let i = -1;
    for (let j = 0; j < 9; j++) {
      const jmod = j % 3;
      if (jmod === 0) {
        matrix.push([]);
        winCells.push([]);
        i++;
      }
      cells.push([i, jmod]);
      matrix[i][jmod] = "";
      winCells[i][jmod] = false;
    }
  }

  init();

  function changeCell(x, y, value) {
    matrix[x][y] = value;
  }

  function reboot() {
    setTimeout(() => {
      phase = phases.play1;
      matrix = [];
      cells = [];
      winCells = [];
      activateShowBegin();
      init();
    }, 3000);
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

    reboot();
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
      
      winCells[0][0] = true;
      if (value === matrix[0][1] && value === matrix[0][2]) {
        
        winCells[0][1] = true;
        winCells[0][2] = true;
      } else if(value === matrix[1][0] && value === matrix[2][0]) {
        
        winCells[1][0] = true;
        winCells[2][0] = true;
      } else {
        
        winCells[1][1] = true;
        winCells[2][2] = true;
      }

      return endGame(value);
    }

    value = matrix[0][1];
    if (value !== "" && value === matrix[1][1] && value === matrix[2][1]) {
      
      winCells[0][1] = true;
      winCells[1][1] = true;
      winCells[2][1] = true;
      return endGame(value);
    }

    value = matrix[0][2];
    if (
      value !== "" &&
      ((value === matrix[1][1] && value === matrix[2][0]) ||
        (value === matrix[1][2] && value === matrix[2][2]))
    ) {
      
      winCells[0][2] = true;
      if (value === matrix[1][1] && value === matrix[2][0]) {
        
        winCells[1][1] = true;
        winCells[2][0] = true; 
      } else {
        
        winCells[1][2] = true;
        winCells[2][2] = true;
      }

      return endGame(value);
    }

    value = matrix[1][0];
    if (value !== "" && value === matrix[1][1] && value === matrix[1][2]) {
      
      winCells[1][0] = true;
      winCells[1][1] = true;
      winCells[1][2] = true;
      return endGame(value);
    }
    
    value = matrix[2][0];
    if (value !== "" && value === matrix[2][1] && value === matrix[2][2]) {
      
      winCells[2][0] = true;
      winCells[2][1] = true;
      winCells[2][2] = true;
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

  function activateShowBegin() {
    showBegin = true;
    setTimeout(() => {
      showBegin = false;
    }, 2000);
  }

  activateShowBegin();
</script>

<div class="game">
  {#if showBegin}
    <p class="text begin">Begin Game!</p>
  {:else}
    <div class="container">
      <header>
        <h2 class="text">
          {#if phase === phases.end}
          <span class="over">The game is over!</span>
          {:else}
          <span>Turn: </span>
          {/if}
          {getHeader()}
        </h2>
        <div class="btn-container">
          <button class="player" on:click={onResetClick}>Reset Game!</button>
        </div>
      </header>

      <div class="board">
        {#each cells as cell, i (i)}
          <Cell
            idx={cell}
            value={matrix[cell[0]][cell[1]]}
            win={winCells[cell[0]][cell[1]]}
            on:cellTouch={onCellTouch}
          />
        {/each}
      </div>
    </div>
  {/if}
</div>

<style>
  .board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 5px;
    background-color: #fff;
    min-height: calc(100vh - 15rem);
    font-size: 12.9vh;
    min-width: calc(100vw - 15rem);
  }

  header {
    display: flex;
    flex-direction: row;
  }

  .player {
    min-width: auto;
    font-size: 1.5rem;
  }

  .btn-container {
    margin-left: auto;
  }

  .text {
    margin-left: 5vw;
  }

  .begin {
    margin-left: 0;
  }

  .container {
    animation: container 2s ease forwards;
  }

  @keyframes container {
    0% {
      transform: scale(.5);
    }
    100% {
      transform: scale(1);
    }
  }

  @media screen and (max-width: 450px) {
    .board {
      font-size: 7vh;
      min-height: calc(100vh - 20rem);
      min-width: calc(100vw - 5rem);
    }
    
    .btn-container {
      margin-left: 0;
    }

    header {
      flex-direction: column-reverse;
    }

    .text {
      margin-left: 0;
      margin-block: 2rem;
    }

    .over {
      display: block;
    }
  }
</style>
