<script>
  import Main from './Main.svelte';
  import Select from './Select.svelte';
  import Game from './Game.svelte';

  const screens = Object.freeze({
    'Main': 'Main',
    'Select': 'Select',
    'Game': 'Game'
  });

  let screen = screens.Main;
  let selectedPlayers = 0;
  let player1 = '';

  function onSelPlayers({ detail: { type } }) {
    selectedPlayers = type;
    screen = screens.Select;
  }

  function onSelPlayer1({ detail: { type } }) {
    player1 = type;
    screen = screens.Game;
  }

  function reset() {
    screen = screens.Main;
  }
</script>

<main>
  <section class="tic-tac-toe">
    {#if screen !== screens.Game}
      <header class="header">
        <h1 class="title">Tic Tac Toe Game!</h1>
      </header>
    {/if}
    
    <div class="container">
      {#if screen === screens.Main}
        <Main on:selPlayers={onSelPlayers} />
      {:else if screen === screens.Select}
        <Select on:play={onSelPlayer1} />
      {:else if screen === screens.Game}
        <Game 
          selectedPlayers={selectedPlayers} 
          player1={player1} 
          on:reset={reset}
          />
      {/if}
    </div>
  </section>
</main>

<style>
  .title {
    font-size: 5rem;
    font-style: italic;
    text-shadow: 2px 2px 2px #fff, -2px -2px 2px #fff;
  }
  .container {
    min-height: calc(100vh - 14rem);
    display: flex;
    justify-content: center;
    align-items: center;
    animation: container 1.5s ease forwards;
  }

  @keyframes container {
    0% {
      transform: scale(0);

    }
    100% {
      transform: scale(1);
    }
  }

  @media screen and (max-width: 450px) {
    .container {
      min-height: calc(100vh - 25rem);
    }
  }
</style>
