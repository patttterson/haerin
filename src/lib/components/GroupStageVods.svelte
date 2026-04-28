<script lang="ts">
	import thumb2758159131 from '$lib/assets/thumbnails/thumb2758159131.jpg';
	import thumb2758159128 from '$lib/assets/thumbnails/thumb2758159128.jpg';
	import thumb2758159130 from '$lib/assets/thumbnails/thumb2758159130.jpg';
	import thumb2758159129 from '$lib/assets/thumbnails/thumb2758159129.jpg';

	interface Vod {
		twitchVideoId: number | null;
		thumbnailUrl: string | null;
		gameNumber: number;
		matchId: number | null;
	}

	const vods: Vod[] = [
		{ twitchVideoId: null, thumbnailUrl: null, gameNumber: 1, matchId: 9350856 },
		{ twitchVideoId: 2758159131, thumbnailUrl: thumb2758159131, gameNumber: 2, matchId: 9351763 },
		{ twitchVideoId: 2758159128, thumbnailUrl: thumb2758159128, gameNumber: 3, matchId: 9353622 },
		{ twitchVideoId: 2758159130, thumbnailUrl: thumb2758159130, gameNumber: 4, matchId: 9354952 },
		{ twitchVideoId: 2758159129, thumbnailUrl: thumb2758159129, gameNumber: 5, matchId: null }
	];
</script>

<div class="vod-grid">
	{#each vods as vod (vod.gameNumber)}
		<div class="vod-card">
			{#if vod.twitchVideoId !== null}
				<a
					class="vod-thumb"
					href="https://www.twitch.tv/videos/{vod.twitchVideoId}"
					target="_blank"
					rel="external noopener"
					style={vod.thumbnailUrl ? `background-image: url('${vod.thumbnailUrl}')` : ''}
				>
					<span class="play-icon">▶</span>
				</a>
			{:else}
				<div class="vod-thumb vod-thumb--unavailable">
					<span class="play-icon">▶</span>
				</div>
			{/if}
			<div class="vod-meta">
				<span class="vod-title">seed #{vod.gameNumber}</span>
				{#if vod.matchId !== null}
					<a
						class="vod-timeline"
						href="https://mcsrranked.com/stats/lesbianpatty/{vod.matchId}?matches=private&sort=newest"
						target="_blank"
						rel="external noopener">timeline →</a
					>
				{:else}
					<span class="vod-timeline--unavailable">timeline unavailable</span>
				{/if}
			</div>
		</div>
	{/each}
</div>

<style>
	.vod-grid {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}
	.vod-card {
		background: var(--color-surface);
		border: 1px solid var(--color-border);
		border-radius: 10px;
		display: flex;
		align-items: center;
		gap: 1rem;
		padding: 0.75rem 1rem;
		transition: border-color 0.15s;
	}
	.vod-card:hover {
		border-color: var(--color-accent);
	}
	.vod-thumb {
		width: 80px;
		aspect-ratio: 16/9;
		background: var(--color-surface-2);
		background-size: cover;
		background-position: center;
		border-radius: 6px;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
		position: relative;
	}
	.vod-thumb::after {
		content: '';
		position: absolute;
		inset: 0;
		background: var(--color-overlay);
		border-radius: 6px;
		backdrop-filter: blur(2px);
	}
	.vod-thumb .play-icon {
		position: relative;
		z-index: 1;
	}
	.vod-thumb--unavailable {
		cursor: default;
	}
	.vod-thumb--unavailable .play-icon {
		color: var(--color-text-disabled);
	}
	.play-icon {
		color: var(--color-accent);
		font-size: 0.9rem;
	}
	.vod-meta {
		display: flex;
		flex-direction: column;
		gap: 0.2rem;
	}
	.vod-title {
		font-size: 0.9rem;
		font-weight: 600;
		color: var(--color-text);
	}
	.vod-timeline {
		font-size: 0.75rem;
		color: var(--color-accent);
		text-decoration: none;
	}
	.vod-timeline:hover {
		text-decoration: underline;
	}
	.vod-timeline--unavailable {
		font-size: 0.75rem;
		color: var(--color-text-disabled);
	}
</style>
