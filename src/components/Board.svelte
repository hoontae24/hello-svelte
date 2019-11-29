<!-- Board.svelte -->
<script>
	import Item from './Item.svelte'
	import { boards, items } from '../stores'
	import { onMount } from 'svelte'
	import Sortable from 'sortablejs'

	export let board = null
	$: _items = $items.filter(item => item.boardId === board.id)

	const handleAddItem = () => items.add(board.id)
	const handleRemoveItem = e => items.remove(e.detail)
	const handleUpdateItem = e => items.update(e.detail)
	const handleSortItem = e => {
		const target = $items.find(item => item.id === e.item.id)
		const allItems = $items.filter(item => item.id !== e.item.id)
		const _items = allItems.filter(item => item.boardId === e.to.id)
		target.boardId = e.to.id
		_items.splice(e.newIndex, 0, target)

		const newItems = allItems
			.filter(item => item.boardId !== e.to.id)
			.concat(_items)
		items.set(newItems)
	}

	let list = null
	onMount(() => {
		if (list) {
			new Sortable(list, {
				group: 'board',
				handle: '.drag-handle',
				fallbackClass: 'sortable-fallback',
				animation: 300,
				onEnd: handleSortItem,
			})
		}
	})
</script>

{#if board}
	<div class="board wrapper card">
		<div class="card-body">
			<div class="meta">
				<h5 class="card-title">
					{board.title}
				</h5>
				<span class="count text-secondary">{_items.length}</span>
			</div>
			<div id={board.id} class="item-list" bind:this={list}>
			{#each _items as item (item.id)}
				<Item
					{item}
					on:update={handleUpdateItem}
					on:remove={handleRemoveItem}
				/>
			{/each}
			</div>
			<div class="card-action">
				<button class="add-item btn" on:click={handleAddItem}><i class="fas fa-plus"></i></button>
			</div>
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

	.item-list {
		padding: 16px 0px;
	}

	.add-item {
		float: right;
		width: 35px;
		height: 35px;
		padding: 0;
	}

</style>