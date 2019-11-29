<!-- Item.svelte -->
<script>
	import { createEventDispatcher, afterUpdate } from 'svelte'
	const dispatch = createEventDispatcher()

	export let item = null
	let hovering = false
	let isEditable = false

	const handleRemoveItem = () => dispatch('remove', item)
	const handleUpdateItem = () => dispatch('update', item)
	const handleToggleEditable = (force) => {
		isEditable = force !== undefined ? force : !isEditable
		if (!isEditable) handleUpdateItem()
	}
	const handleToggleDone = () => {
		item.done = !item.done
		handleUpdateItem()
	}
</script>

{#if item}
<div
	id={item.id}
	class="item card p-1 mb-2 shadow-sm"
	class:shadow={hovering}
	on:mouseenter={() => hovering = true}
	on:mouseleave={() => hovering = false}
>
	<div class="drag-handle pointer"><i class="fas fa-grip-lines"></i></div>
	{#if isEditable}
	<input
		bind:value={item.title} 
		on:keypress={({key})=> key ==='Enter' && handleToggleEditable(false)}
	/>
	{:else}
	<span
		class="item-title pointer"
		class:item-done={item.done}
		on:click={handleToggleDone}
	>
		{item.title}
	</span>
	{/if}
	<div class="item-action">
		<button class="item-btn btn" on:click={() => handleToggleEditable()}><i class="far fa-edit"></i></button>
		<button class="item-btn btn" on:click={handleRemoveItem}><i class="far fa-trash-alt"></i></button>
	</div>
</div>
{/if}

<style>
	.item {
		transition: box-shadow .25s ease-in;
		flex-direction: row;
		justify-content: space-between;
		flex-wrap: nowrap;
		align-items: center;
	}

	.item input {
		padding: 4px;
		height: 25px;
		width: 160px;
	}

	.pointer {
		cursor: pointer;
	}

	.item-done {
		text-decoration: line-through;
		color: gray;
	}

	.item-btn {
		width: 25px;
		height: 25px;
		padding: 0;
	}
</style>