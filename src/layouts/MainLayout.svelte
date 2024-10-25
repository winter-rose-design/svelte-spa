<script>
	let props = $props();
</script>

<svelte:head>
	<title>{props.metadata?.title}</title>
</svelte:head>

<div>
	<main>
		{@render props.children()}
	</main>
</div>
