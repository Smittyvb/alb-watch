<script>
	export let addr;

	let dataP;
	$: dataP = window.call("getAccount", [addr])
</script>

<style>
	h2 {
		margin: 0;
		font-size: 5rem;
	}
	.balance {
		font-weight: bold;
		font-size: 3rem;
	}
</style>

{#await dataP}
	Loading...
{:then data}
	{#if Object.keys(data).length > 1}
		{#each Object.keys(data) as key}
			{key}: {data[key].balance / 100000} NIM
		{/each}
	{:else}
		<h2>{Object.keys(data)[0]} account</h2>
		<div class="balance">{data[Object.keys(data)[0]].balance / 100000} NIM</div>
	{/if}
{/await}
