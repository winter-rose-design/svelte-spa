<script>
	let props = $props();
</script>

<svelte:head>
	<title>{props.metadata?.title}</title>
</svelte:head>

<div>
	<main>
		<slot></slot>
	</main>
</div>
