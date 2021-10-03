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
  <h2>Player 1!</h2>
  <p>Do you want play with X or with O?</p>
  <button class="player" on:click={() => play('X')}>
    X
  </button>
  <button class="player" on:click={() => play('O')}>
    O
  </button>
</div>
