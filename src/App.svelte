<script>
	// Currently similar Svelte Issues:
	// encapsulated URL sharing https://github.com/sveltejs/sites/issues/330
	// embed ask: https://github.com/sveltejs/sites/issues/93
	// embed ask: https://github.com/sveltejs/sites/issues/140
	// changing content if not forked https://github.com/sveltejs/sites/issues/113

	// This data would be available from REPL
	let name = 'Custom Name for Permalink';
	let data = {
		components: [
			{
				name: 'App',
				type: 'svelte',
				source:
					`<scr` +
					`ipt>
  import "./data.js"
  let name = 'world';
</scr` +
					`ipt>

<h1>Hello {name}!</h1>`,
			},
			{
				name: 'data',
				type: 'js',
				source: `export let data = [0,1,2];`,
			},
		],
	};

	// update the URL with params data
	createPermalink(data, name);

	// now pull in the params data from the URL to update the REPL.
	consumePermalink();

	function createPermalink(data, name) {
		// get existing search params
		const params = new URLSearchParams(decodeURIComponent(window.location.search));
		// update search params with new data and name
		params.set('name', encodeURI(name));
		params.set('data', encodeURI(JSON.stringify(data)));
		// write to URL
		window.history.replaceState({}, '', `${location.pathname}?${params.toString()}`);
	}

	function consumePermalink() {
		//  get search params
		const params = new URLSearchParams(window.location.search);
		const name = decodeURI(params.get('name'));
		const data = JSON.parse(decodeURI(params.get('data')));

		// update REPL with name;
		// update REPL components
		// repl.set({ components: source.components	});

		// just for illustration
		// console.log(data);
		data.components.forEach(({ name, type, source }, idx) => {
			console.log(`Tab ${idx + 1} Name is: ${name}.${type}`);
			console.log(`Tab ${idx + 1} Source is : ${source}`);
		});
	}
</script>
