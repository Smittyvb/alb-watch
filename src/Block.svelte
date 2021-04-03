<script>
	export let num;

	let dataP;
	$: dataP = window.call("getBlockByNumber", [num, true])
</script>

<style>
	h2 {
		font-size: 4rem;
		text-align: center;
		margin: 0;
		padding: 0;
	}
	.nq-notice {
		margin-top: 0;
	}
	.block-data {
		width: 100%;
	}
	.block-data > div {
		margin-bottom: 2rem;
	}
	.block-data .label {
		font-weight: bold;
	}
	.block-data .value {
		font-family: monospace;
		font-size: 90%;
		overflow-wrap: anywhere;
		display: block;
	}
	.nq-card {
		margin-left: auto;
    	margin-right: auto;
		max-width: 110rem;
	}
</style>

{#await dataP}
	Loading block
{:then block}
	{#if block}
		<div class="nq-card">
			<div class="nq-card-header">
				<h2 class="nq-h2">{block.blockType} block</h2>
				<div class="nq-notice">
					<div>
						at {(new Date(block.timestamp)).toLocaleString()}
					</div>
					<div>
						micro #{block.blockNumber}, macro #{block.epoch}
					</div>
				</div>
			</div>
			<div class="nq-card-body">
				<div class="block-data">
					<div>
						<span class="label">Hash</span>
						<span class="value">{block.hash}</span>
					</div>
					<div>
						<span class="label">Extra data</span>
						<span class="value">{block.extra_data ? block.extra_data : "(none)"}</span>
					</div>
					<div>
						<span class="label">Batch</span>
						<span class="value">{block.batch}</span>
					</div>
					<div>
						<span class="label">View number</span>
						<span class="value">{block.viewNumber}</span>
					</div>
					<div>
						<span class="label">Body root</span>
						<span class="value">{block.bodyRoot}</span>
					</div>
					<div>
						<span class="label">State root</span>
						<span class="value">{block.stateRoot}</span>
					</div>
					<div>
						<span class="label">VRF seed</span>
						<span class="value">{block.seed}</span>
					</div>
					<div>
						<span class="label">Justification</span>
						<span class="value">{block.seed}</span>
					</div>
					<div>
						<span class="label">View change justification</span>
						<span class="value">
							{#if block.justification.viewChangeProof}
								{block.justification.viewChangeProof}
							{:else}
								N/A
							{/if}
						</span>
					</div>
					<div>
						<span class="label">Validator slot</span>
						<span class="value">{block.producer.slotNumber}</span>
					</div>
					<div>
						<span class="label">Validator ID</span>
						<span class="value">{block.producer.validatorId}</span>
					</div>
					<div>
						<span class="label">Validator public key</span>
						<span class="value">{block.producer.publicKey}</span>
					</div>
				</div>
				{#if block.fork_proofs.length > 0}
					<div>
						There were fork proofs this block!
					</div>
				{/if}
				<div>
					<h3 class="nq-h3">{block.transactions.length} transcations</h3>
					TODO
				</div>
			</div>
		</div>
	{:else}
		Block not found.
	{/if}
{/await}
