<script lang="ts">
	import './layout.css';
	import './fonts.css';
	import { dev } from '$app/environment';

	let { children } = $props();

	const title = 'no%' + (dev ? ' [dev]' : '');

	let fiveCount = 0;
	let lastFiveTime = 0;

	function handleKeydown(e: KeyboardEvent) {
		const now = Date.now();
		if (e.key === '5') {
			if (fiveCount === 1 && now - lastFiveTime < 600) {
				fiveCount = 2;
			} else {
				fiveCount = 1;
			}
			lastFiveTime = now;
		} else if (e.key === 'Enter' && fiveCount === 2) {
			window.location.href = '/55.gif';
			fiveCount = 0;
		} else {
			fiveCount = 0;
		}
	}
</script>

<svelte:head>
	<meta name="viewport" content="width=device-width, initial-scale=1" />
	<title>{title}</title>
	<meta
		name="description"
		property="og:description"
		content="a community tournament for runners who hate percentages"
	/>
	<meta name="theme-color" content="#f72585" />
	<link rel="icon" href="/favicon.svg" type="image/svg+xml" />
	<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
	<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
	<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
	<link rel="manifest" href="/site.webmanifest" />

	<link rel="dns-prefetch" href="https://ilovecatgirls.xyz/" />
	<link rel="dns-prefetch" href="https://nmsr.nickac.dev/" />
</svelte:head>

<svelte:window onkeydown={handleKeydown} />

<div id="root">
	{@render children()}
</div>

<style>
	:global(body) {
		font-family: 'Comfortaa', system-ui, sans-serif;
		background-color: var(--color-bg);
		color: var(--color-text);
		display: flex;
		justify-content: center;
		align-items: flex-start;
		min-height: 100vh;
		margin: 0;
	}

	:global(::selection),
	:global(::-moz-selection) {
		background: var(--color-accent);
		color: var(--color-bg);
	}

	#root {
		width: 100%;
		min-height: 100vh;
		display: flex;
		flex-direction: column;
	}
</style>
