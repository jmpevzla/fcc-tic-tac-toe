<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  function play(type) {
    dispatch('play', {
      type
    });
  }
</script>

<div class="selectXO">
  <div class="text">
    <h2>Player 1!</h2>
    <p>Do you want play <span>with X or with O?</span></p>
  </div>
  
  <button class="player" on:click={() => play('X')}>
    X
  </button>
  <button class="player" on:click={() => play('O')}>
    O
  </button>
</div>

<style>
  @media screen and (max-width: 450px) {
    span {
      display: block;
    }
  }
</style>
