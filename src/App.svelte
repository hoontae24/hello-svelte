<!-- App.svelte -->
<script>
	import Appbar from './components/Appbar.svelte'  
  import Board from './components/Board.svelte'
  import { boards, items } from './stores'
</script>

<Appbar />
<div class="container">
  {#each $boards as board (board.id)}
    <div class="item">
      <Board board={board} />
    </div>
  {/each}
</div>

<style>
	.container {
    overflow: auto;
    display: flex;
	}
  .container .item {
    min-width: 300px;
    width: 300px;
    flex-grow: 1;
    margin: 32px;
  }
</style>
