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
				<h2 class="nq-h2">{#if block.is_election_block}election {/if}{block.blockType} block #{#if block.blockType === "micro"}{block.blockNumber}{:else}{block.epoch}{/if}</h2>
				<div class="nq-notice">
					<div>
						at {(new Date(block.timestamp)).toLocaleString()}
						{#if block.blockType === "macro"}
							with {block.justification.sig.signers.length} signers ({(block.justification.sig.signers.length / 512 * 100).toFixed(2)}%)
						{/if}
					</div>
					<div>
						{#if block.blockType === "micro"}
							Part of epoch #{block.epoch}
						{/if}
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
					{#if block.blockType === "micro"}
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
					{:else}
						<div>
							<span class="label">Signature</span>
							<span class="value">{block.justification.sig.signature}</span>
						</div>
					{/if}
				</div>
				{#if block.fork_proofs && block.fork_proofs.length > 0}
					<div>
						There were fork proofs this block!
					</div>
				{/if}
				{#if block.blockType === "macro"}
					Scheudled slots: TODO
					Lost rewards: TODO
				{/if}
				<div>
					{#if block.transactions}
						<h3 class="nq-h3">{block.transactions.length} transcations</h3>
						TODO
					{:else}
						No transactions in macro blocks.
					{/if}
				</div>
			</div>
		</div>
	{:else}
		Block not found.
	{/if}
{/await}
