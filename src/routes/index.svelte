<script context="module">
	export const metadata = $state({
		title: 'Index',
	});

	export async function load(params) {
		return {
			name: 'Svelte 5',
		};
	}
</script>

<script>
	let props = $props();

	metadata.title = 'Hi';
</script>

<h1>{props?.data?.name}</h1>

<ul>
	<li>
		<a href="/">Index</a>
	</li>
	<li>
		<a href="/about">About</a>
	</li>
	<li>
		<a href="/users">Users</a>
	</li>
</ul>
