<script lang="ts">
	import { dev } from '$app/environment';
	import { resolve } from '$app/paths';
</script>

<div class="page">
	<header>
		<a class="back" href={resolve('/')}>← no<span class="pct">%</span> invitational</a>
		{#if dev}
			<p class="dev-label">DEV BUILD</p>
		{/if}
	</header>

	<main>
		<div class="hero">
			<p class="eyebrow">a note on mental</p>
			<h1>don't forfeit.</h1>
			<p class="context">
				no% invitational has a forfeit rate cap as part of its eligibility requirements. the whole
				point of the tournament is playing through bad seeds instead of resetting them.
				<span class="text-white"
					>if forfeiting when things go wrong is your habit, this probably isn't the right
					tournament for you.</span
				>
			</p>
		</div>

		<div class="points">
			<div class="point-card point-card--accent">
				<span class="point-num">01</span>
				<div class="point-body">
					<h2 class="point-title">your opponent can still lose</h2>
					<p class="point-text">
						No lead is safe in Ranked. Your opponent could miss the lava in PR, misplace obsidian on
						the second portal, fail the zero, or even disconnect, and suddenly you're ahead. The
						moment you forfeit, you hand them a win they haven't earned yet. You can come back from
						a mistake, but you can't come back from a forfeit.
					</p>
				</div>
			</div>

			<div class="point-card">
				<span class="point-num">02</span>
				<div class="point-body">
					<h2 class="point-title">it's unsportsmanlike</h2>
					<p class="point-text">
						Your opponent showed up to play a full match. Forfeiting before the run is clearly over
						disrespects their time, their effort, and the game itself. On top of that, forfeiting
						voids the run for them too. If they were on pace for a personal best, that time doesn't
						count. You're not just giving up your own run, you're taking theirs.
					</p>
				</div>
			</div>

			<div class="point-card">
				<span class="point-num">03</span>
				<div class="point-body">
					<h2 class="point-title">"it's necessary for my mental" just stop queuing</h2>
					<p class="point-text">
						If your mental is bad enough to forfeit, it's bad enough to log off. You can't use both
						as an excuse at the same time. Most of the time "necessary for my mental" just means
						wanting to skip to the next game, which isn't really a mental argument, it's just
						impatience. Take a break if you need one, but forfeiting and immediately requeuing
						doesn't actually help anything.
					</p>
				</div>
			</div>
		</div>

		<p class="disclaimer">
			this is the opinion of the organizer, pattersonuwu, not a reflection of the views of the rest
			of the no% invitational team.
		</p>

		<div class="footer-note">
			<p>glhf, don't ff</p>
		</div>
	</main>
</div>

<style>
	.page {
		width: 100%;
		max-width: 720px;
		margin: 0 auto;
		padding: 0 1.5rem;
		min-height: 100vh;
		display: flex;
		flex-direction: column;
	}

	header {
		padding: 1.5rem 0;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: 1px solid var(--color-border);
	}

	.back {
		font-size: 0.85rem;
		font-weight: 600;
		color: var(--color-text-muted);
		text-decoration: none;
		transition: color 0.15s;
		letter-spacing: -0.01em;
	}
	.back:hover {
		color: var(--color-accent);
	}
	.pct {
		color: var(--color-accent);
	}

	.dev-label {
		font-size: 0.75rem;
		font-weight: 700;
		font-family: monospace;
		letter-spacing: 0.12em;
		text-transform: uppercase;
		color: var(--color-text-disabled);
		margin: 0;
	}

	main {
		flex: 1;
		padding: 1rem 0 3rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.hero {
		display: flex;
		flex-direction: column;
		gap: 0.6rem;
	}

	.eyebrow {
		font-size: 0.7rem;
		text-transform: uppercase;
		letter-spacing: 0.14em;
		color: var(--color-accent);
		font-weight: 700;
		margin: 0;
	}

	h1 {
		font-size: clamp(2.5rem, 8vw, 4rem);
		font-weight: 700;
		letter-spacing: -0.04em;
		line-height: 1;
		margin: 0;
		color: var(--color-text);
	}

	.context {
		font-size: 0.85rem;
		line-height: 1.6;
		color: var(--color-text-muted);
		border-left: 2px solid var(--color-accent);
		padding-left: 0.85rem;
		margin: 0;
	}

	.disclaimer {
		font-size: 1rem;
		color: var(--color-text-disabled);
		font-style: italic;
		margin: 0;
	}

	.points {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.point-card {
		display: grid;
		grid-template-columns: 3.5rem 1fr;
		background: var(--color-surface);
		border: 1px solid var(--color-border);
		border-radius: 12px;
		padding: 1.5rem 1.75rem;
		transition: border-color 0.15s;
	}
	.point-card:hover {
		border-color: var(--color-text-disabled);
	}
	.point-card--accent {
		border-left: 3px solid var(--color-accent);
	}
	.point-card--accent:hover {
		border-color: var(--color-accent);
	}

	.point-num {
		font-size: 0.65rem;
		font-weight: 700;
		font-family: monospace;
		letter-spacing: 0.1em;
		color: var(--color-text-disabled);
		padding-top: 0.25rem;
	}

	.point-body {
		display: flex;
		flex-direction: column;
		gap: 0.6rem;
	}

	.point-title {
		font-size: 1rem;
		font-weight: 700;
		letter-spacing: -0.02em;
		color: var(--color-text);
		margin: 0;
	}

	.point-text {
		font-size: 0.9rem;
		line-height: 1.65;
		color: var(--color-text-muted);
		margin: 0;
	}

	.footer-note {
		text-align: center;
		padding: 1.5rem 0 2rem;
		border-top: 1px solid var(--color-border);
	}
	.footer-note p {
		margin: 0;
		font-size: 0.8rem;
		color: var(--color-text-disabled);
		letter-spacing: 0.06em;
	}

	@media (max-width: 480px) {
		.point-card {
			grid-template-columns: 1fr;
			gap: 0.5rem;
		}
	}
</style>
