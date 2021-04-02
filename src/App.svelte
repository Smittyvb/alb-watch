<script>
	import { Client } from "rpc-websockets";
	import States from "./states.ts";

	const RPC_WS_SERVER = "ws://seed1.nimiq.local:8648/ws";

	let socket = new Client(RPC_WS_SERVER, {
		max_reconnects: 0, // unlimited
	});
	let state = States.CONNECTING;
	socket.on("open", () => {
		state = States.CONNECTED;
		setTimeout(() => socket.call("getTransactionsByHash", ["67b4023a6cc840acaa5b269201479ee46e1678a7ef81604e735a3477b0de2d1e"]).then(num => console.log("num", num)), 1000);
	});
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
	<h1>Albawatch</h1>
	<span class="conn-state">
		{state.toString()}
	</span>
</nav>
<main>
	<div class="content">
		<input type="text" id="search-input" placeholder="Search accounts, blocks, and transactions">
	</div>
</main>
