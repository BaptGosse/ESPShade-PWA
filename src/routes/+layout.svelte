<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import Header from '$lib/components/Header.svelte';

	let { children } = $props();
	let currentUser = null;
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<Header {currentUser} />

<main>
  {@render children?.()}
</main>

<style>
  main {
    background: white;
    min-height: calc(100vh - 70px); /* 70px = hauteur header */
    padding: 20px;
  }
</style>