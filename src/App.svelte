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
  <section class="game">
    <header class="header">
      <h1>Tic Tac Toe Game!</h1>
    </header>
    
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
  </section>
</main>

<style>


</style>
