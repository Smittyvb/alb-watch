<script>
	export let search;

	const TYPES = {
		addr: {
			regex: /^\s*(N|n)(Q|q)\d\d((\s|\+)?[0-9a-zA-Z]{4}){8}\s*$/,
		},
		block: {
			regex: /^\s*\d{1,15}\s*$/
		},
		tx: {
			regex: /^\s*[a-fA-F0-9]{64}\s*$/,
		},
		homepage: {
			regex: /^\s*$/,
		}
	};

	function parse(s) {
		for (const type in TYPES) {
			const typeConfig = TYPES[type];
			if (!s.match(typeConfig.regex)) continue;
			return type;
		}
		return "none";
	}

	let parsed;
	$: parsed = parse(search);
</script>

{parsed}
