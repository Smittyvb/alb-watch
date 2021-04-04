<script>
	export let data;
	export let full = false;
	// data is like {"blockNumber":423,"confirmations":56,"data":"","fee":0,"flags":0,"from":"NQ55 X103 EJXG 3S2U 3JNE L779 560T 3LNE 7S60","hash":"3bd6a466ea48cdc506eb78bf977537896bdbcfb673efa3d9065799be7ece80dc","proof":"d11f809f2b4035ee2b5cc24f4f1630a904767e33740ed906730d12a3cf0fd67800f971a252da239875225f6bbae64056ce2bcb86f477459a12b0e32d2d66b9f75c7e5bfbadd84ba4718aaece810a68b9f4ff646f6be9eef131c0cf35b9e61f780e","timestamp":1617575010402,"to":"NQ58 VL4Q ALUE CU9N NUEA 1NYU EY6P HDCA 7GUE","validityStartHeight":421,"value":100000000}
</script>

<style>
	code {
		display: block;
	}
	.tx {
		text-align: center;
		border: 2px solid rgb(80, 80, 80);
		border-radius: 1rem;
		margin-bottom: 1rem;
	}
	.hash {
		font-size: 90%;
		color: rgb(77, 77, 77);
	}
</style>

<div class="tx">
	<div class="hash">
		{data.hash}
	</div>
	<div>{data.value / 100000} NIM ({data.fee} luna fee) transferred from <code>{data.from}</code> to <code>{data.to}</code> </div>
	<div>
		Extra data: {data.data || "none"}
	</div>
	{#if data.flags}
		Flags: <code>{data.flags.toString(2)}</code>
	{/if}
</div>
