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

<div class="cell" on:click={cellTouch}>{value}</div>

<style>
   .cell {
    background-color: black;
    color: white;
    min-height: 15rem;
    text-align: center;
    padding: 2rem;
    font-size: 10rem;
  }
</style>