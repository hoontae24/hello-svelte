<!-- Item.svelte -->
<script>
	export let item = null
	let hovering = false
</script>

{#if item}
<div
	class="item card p-1 mb-2 shadow-sm"
	class:shadow={hovering}
	on:mouseenter={() => hovering = true}
	on:mouseleave={() => hovering = false}
>
	<span>
		{item.title}
	</span>
</div>
{/if}

<style>
	.item {
		transition: box-shadow .25s ease-in;
	}
</style>