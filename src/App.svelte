<script>
	import States from "./states.ts";
	import Thing from "./Thing.svelte";

	const RPC_WS_SERVER = "ws://seed1.nimiq.local:8648/ws";
	const EXP_BACKOFF_START = 1000;

	let rpcId = 1;
	let socket, openCbs;
	let expBackoff = EXP_BACKOFF_START;

	let idCallbacks = Object.create(null);
	let state = States.CONNECTING;
	function createSocket() {
		openCbs = [];
		socket = new WebSocket(RPC_WS_SERVER);
		socket.addEventListener("open", () => {
			state = States.CONNECTED;
			expBackoff = EXP_BACKOFF_START;
			let beforeId = rpcId;
			for (const cbId in idCallbacks) {
				idCallbacks[cbId].retry();
				delete idCallbacks[cbId];
			}
			openCbs.forEach(cb => cb());
		});
		socket.addEventListener("message", async e => {
			let text = e.data;
			if (typeof e !== "string") {
				// text is a blob
				text = await text.text();
			}
			const json = JSON.parse(text);
			if (json.jsonrpc !== "2.0") throw new Error("unknown JSON-RPC version");
			if (idCallbacks[json.id]) {
				idCallbacks[json.id].done(json.result);
				delete idCallbacks[json.id];
			} else {
				console.warn("got message without handler for ID", json);
			}
		});
		socket.addEventListener("close", e => {
			state = States.WAITING;
			setTimeout(() => createSocket(), expBackoff);
			if (expBackoff < 10000) expBackoff *= 2;
		});
	}
	createSocket();

	function call(method, params) {
		if (state !== States.CONNECTED) {
			return new Promise((resolve, _reject) => {
				const handler = () => {
					socket.removeEventListener("open", handler);
					call(method, params).then(resolve);
				};
				socket.addEventListener("open", handler);
			});
		}
		socket.send(JSON.stringify({
			jsonrpc: "2.0", method: "getBlockNumber", params: [], id: ++rpcId,
		}));
		return new Promise((resolve, reject) => {
			idCallbacks[rpcId] = {
				done: resolve,
				retry: () => call(method, params),
			};
		});
	}

	(async () => {
		console.log("bn", await call("getBlockNumber", []))
	})();

	function nameState(state) {
		if (state === States.CONNECTING) return "Connecting...";
		if (state === States.CONNECTED) return "Connected";
		if (state === States.WAITING) return "Error, waiting to reconnect";
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
