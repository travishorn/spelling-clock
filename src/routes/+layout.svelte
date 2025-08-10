<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/clock-icon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<title>Clock</title>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children?.()}
