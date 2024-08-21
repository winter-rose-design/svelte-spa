<script>
	import navaid from 'navaid';
	import MainLayout from '@/layouts/MainLayout.svelte';

	let state = $state({
		data: null,
		params: null,
		metadata: null,
		component: null,
	});

	const router = navaid('/', async (uri) => {
		const { default: component } = await import('./errors/404.svelte');

		state = { component };
	});

	const routes = Object.entries(import.meta.glob('@/routes/**/*.svelte')).map(([path, module]) => {
		const [, match] = /routes(.+)\.svelte$/.exec(path);

		path = match.replace('/index', '').replace('@', ':');

		return {
			module,
			path: path === '' ? '/' : path,
		};
	});

	for (const { path, module } of routes) {
		router.on(path, (params) => {
			const navigate = async (params) => {
				try {
					const { metadata, load, default: component } = await module();
					const data = await load?.(params);

					state = { component, params, data, metadata };
				} catch (error) {
					const { default: component } = await import('./errors/500.svelte');

					state = { component };
				}
			};

			if (!document.startViewTransition) {
				navigate(params);
				return;
			}

			document.startViewTransition(() => navigate(params));
		});
	}

	router.listen();
</script>

<MainLayout metadata={state.metadata}>
	<state.component params={state.params} data={state.data}></state.component>
</MainLayout>
