<script>
	import { Client } from "rpc-websockets";
	import States from "./states.ts";
	import Thing from "./Thing.svelte";

	const RPC_WS_SERVER = "ws://seed1.nimiq.local:8648/ws";

	let socket = new Client(RPC_WS_SERVER, {
		max_reconnects: 0, // unlimited
	});
	let state = States.CONNECTING;
	socket.on("open", () => {
		state = States.CONNECTED;
		setTimeout(() => socket.call("blockNumber", {}, 1000, {binary: true}).then(num => console.log("num", num)), 1000);
	});

	function nameState(state) {
		if (state === States.CONNECTING) return "Connecting...";
		if (state === States.CONNECTED) return "Connected";
	}

	let search = "";
</script>

<style>
	.conn-state {
		float: right;
	}
	nav {
		color: black;
		padding: 0.5rem;
	}
	h1 {
		display: inline;
	}
	main {
		display: flex;
		flex-direction: row;
		justify-content: center;
		width: 100%;
		margin-top: 1rem;
	}
	.content {
		padding: 1rem;
		box-sizing: border-box;
		width: 100%;
		max-width: 100rem;
	}
	#search-input {
		width: 100%;
		font-family: monospace;
		padding: 1.25rem;
	}
</style>

<nav class="nq-gold-bg">
	<h1 class="nq-h1">Albawatch</h1>
	<span class="conn-state">
		{nameState(state)}
	</span>
</nav>
<main>
	<div class="content">
		<input type="text" id="search-input" placeholder="Search accounts, blocks, and transactions" bind:value={search}>
		<Thing {search} />
	</div>
</main>
