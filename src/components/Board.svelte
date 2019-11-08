<!-- Board.svelte -->
<script>
	import Item from './Item.svelte'

	export let board = null
	export let items = []
</script>

{#if board}
	<div class="board wrapper card">
		<div class="card-body">
			<div class="meta">
				<h5 class="card-title">
					{board.title}
				</h5>
				<span class="count text-secondary">{items.length}</span>
			</div>
			{#each items as item (item.id)}
				<Item {item}/>
			{/each}
		</div>
	</div>
{/if}

<style>
	.board.wrapper {
		background-color: #ebebeb;
	}

	.meta {
		padding: 0 4px;
		display: flex;
		justify-content: space-between;
		align-content: center;
	}

</style>