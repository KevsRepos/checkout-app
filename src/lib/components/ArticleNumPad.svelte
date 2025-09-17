<script lang="ts">
let {articlePrice = $bindable(), articleQuantity = $bindable(), hasDeposit = $bindable(), articleName, ...rest} = $props();

const toggle = (quantity: number) => {
	if(articleQuantity === quantity) {
		articleQuantity = 0;
	} else {
		articleQuantity = quantity;
	}
}
</script>

<div {...rest}>
	<div class="px-1">
		<span class="uppercase">{articleName}</span>
		<input class="bg-gray-100 w-15" type="number" bind:value={articlePrice}/>ct
		<input type="checkbox" bind:checked={hasDeposit}/>
		Glaspfand
	</div>
	<div class="grid grid-cols-5">
		{#each { length: 30 }, quantity}
			<button 
				class={["m-1 p-2", articleQuantity === quantity + 1 ? 'bg-green-800 text-white' : 'bg-gray-200']} 
				onpointerdown={() => toggle(quantity + 1)}
			>
				{quantity + 1}
			</button>
		{/each}
	</div>
</div>