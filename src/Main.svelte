<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  
  function selPlayers(type) {
    dispatch('selPlayers', {
      type
    });
  }
</script>

<div class="main">
  <p class="text">
    Do you want play now?
  </p>
  <div class="buttons">
    <button class="player" on:click={() => selPlayers(1)}>
      1 Player vs CPU!
    </button>
    <button class="player" on:click={() => selPlayers(2)}>
      2 Players!
    </button>
  </div>
</div>

<style>
  .buttons {
    display: flex;
  }

  @media screen and (max-width: 450px) {
    
    .buttons {
      flex-direction: column;
    }

  }
</style>