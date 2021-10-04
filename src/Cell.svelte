<script>
  export let value = '';
  export let idx = [];
  
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  function cellTouch() {
    dispatch('cellTouch', {
      idx
    });
  }
</script>

<div class="cell" on:click={cellTouch}>
  <p class={value !== '' && 'active'}>{value}</p>
</div>

<style>
   .cell {
    background-color: black;
    color: white;
    text-align: center;
    padding-inline: 2rem;
    padding-top: 1.2rem;
    min-height: 18vh;
  }

  .active {
    animation: cell 1s ease-in forwards;
  }

  @keyframes cell {
    0% {
      transform: scale(0.5);
    }
    100% {
      transform: scale(1);
    }
  }
  
  @media screen and (max-width: 450px) {
    .cell {
      padding-top: 6rem;
    }
  }

</style>