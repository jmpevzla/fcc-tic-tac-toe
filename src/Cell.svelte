<script>
  export let value = '';
  export let idx = [];
  export let win = false;
  
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  function cellTouch() {
    dispatch('cellTouch', {
      idx
    });
  }
</script>

<div class={win ? 'cell win' : 'cell'} on:click={cellTouch}>
  <p class={value !== '' && 'active'}>{value}</p>
</div>

<style>
   .cell {
    background-color: #000;
    color: #fff;
    text-align: center;
    padding-inline: 2rem;
    padding-top: 1.2rem;
    min-height: 18vh;
  }

  .win > p {
    color: #f00;
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