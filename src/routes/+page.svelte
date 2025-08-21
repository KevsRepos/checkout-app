<script lang="ts">
import ArticleNumPad from "$lib/components/ArticleNumPad.svelte";

let regularDrinkPrice: number = $state(210);

let cornPrice: number = $state(150);

let mixedPrice: number = $state(350);

let depositPrice = $state(100);

let regularDrinkQuantity = $state(0);

let cornQuantity = $state(0);

let mixedQuantity = $state(0);

let depositInput: string = $state('');

let depositReceived = $derived(parseInt(depositInput) || 0);

let moneyInput: string = $state('');

let moneyReceived = $derived(parseFloat(moneyInput) * 100 || 0);

const amountRegularDrink = $derived((regularDrinkQuantity * regularDrinkPrice) + (regularDrinkQuantity * depositPrice));

const amountCorn = $derived(cornQuantity * cornPrice);

const amountMixed = $derived((mixedQuantity * mixedPrice) + (mixedQuantity * depositPrice));

const amountDeposit = $derived(depositReceived * depositPrice);

const total: number = $derived((amountRegularDrink + amountCorn + amountMixed - amountDeposit) / 100);

const toReturn = $derived((moneyReceived - (amountRegularDrink + amountCorn + amountMixed - amountDeposit)) / 100);

const resetAll = () => {
	regularDrinkQuantity = 0;
	cornQuantity = 0;
	mixedQuantity = 0;
	depositInput = '';
	moneyInput = '';
}
</script>

<div class="grid grid-cols-3 border-b [&>*]:px-2 [&>*]:pb-2">
	<ArticleNumPad bind:articlePrice={regularDrinkPrice} bind:articleQuantity={regularDrinkQuantity} articleName="Getränk 0,2l"/>
	<ArticleNumPad bind:articlePrice={mixedPrice} bind:articleQuantity={mixedQuantity} articleName="Mischgetränk 0,2l" class="border-x"/>
	<ArticleNumPad bind:articlePrice={cornPrice} bind:articleQuantity={cornQuantity} articleName="Korn 0,02l"/>
</div>

<div class="grid grid-cols-2 gap-1 px-1 pt-1 border-b">
	<div>
		<div>
			<div class="flex justify-between items-center">
				<div>
					<span class="uppercase ps-1">Erhaltene Gläser</span>
					<input class="bg-gray-100 w-15" bind:value={depositPrice}/>ct
				</div>
				<div class="text-3xl h-9 min-w-1/4 me-1 flex justify-end items-center gap-2">
					{#if depositInput.length}
						<span>{depositInput} Stk</span>
						<button class="leading-1" onpointerdown={() => depositInput = ''}>❌</button>
					{/if}
				</div>
			</div>
			<div class="grid grid-cols-3">
				{#each { length: 9 }, number}
					<button class="m-1 p-2 bg-gray-200" onpointerdown={() => depositInput += number+1}>{number + 1}</button>
				{/each}
				<button class="m-1 p-2 bg-gray-200 col-span-3" onpointerdown={() => depositInput += '0'}>0</button>
			</div>
		</div>
	</div>

	<div>
		<div>
			<div class="flex justify-between items-center">
				<div class="uppercase ps-1 pt-1">Erhaltene Euro</div>
				<div class="text-3xl h-9 min-w-1/4 me-1 flex justify-end items-center gap-2">
					{#if moneyInput.length}
						<span>{Number(moneyInput).toFixed(2)}€</span>
						<button class="leading-1" onpointerdown={() => moneyInput = ''}>❌</button>
					{/if}
				</div>
			</div>
			<div class="grid grid-cols-3">
				{#each { length: 9 }, number}
					<button class="m-1 p-2 bg-gray-200" onpointerdown={() => moneyInput += number+1}>{number + 1}</button>
				{/each}
				<button class="m-1 p-2 bg-gray-200 col-span-2" onpointerdown={() => moneyInput += '0'}>0</button>
				<button class="m-1 p-2 bg-gray-200 col-span-1" onpointerdown={() => moneyInput += '.'}>.</button>
			</div>
		</div>
	</div>
</div>

<div class="w-full mt-1 px-2 bg-white z-40 flex justify-between">
	<div>
		<div class="bg-gray-400 px-1">TOTAL:</div>
		<div class="bg-gray-300 text-8xl">{total.toFixed(2)}€</div>
	</div>

	{#if toReturn >= 0}
		<div>
			<div class="bg-gray-400 px-1">ZURÜCK:</div>
			<div class="bg-gray-300 text-8xl">{toReturn.toFixed(2)}€</div>
		</div>
	{/if}

	<div class="flex flex-col items-end">
		<div>({amountRegularDrink} + {amountCorn} + {amountMixed} - {amountDeposit}) = {(total * 100).toFixed(0)}</div>
		<button class="h-full w-30 bg-red-500 text-4xl text-white" onpointerdown={resetAll}>C</button>
	</div>
</div>