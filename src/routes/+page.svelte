<script lang="ts">
	import { dev } from '$app/environment';
	import diaPick from '$lib/assets/dia_pick.png';
	import EligibilityChecker from '$lib/components/EligibilityChecker.svelte';
	import GroupStageVods from '$lib/components/GroupStageVods.svelte';
	import Credits from '$lib/components/Credits.svelte';

	const prizes = [
		{
			place: '1st',
			name: 'junesies',
			uuid: '2125698b-0d79-4c4c-9969-8e39430e65fc',
			desc: '6 months',
			value: 54,
			soup: true
		},
		{
			place: '2nd',
			name: '_MonkeyBoy_',
			uuid: '0234262f-0ae0-4d6a-8f09-22f30fb5ce9e',
			desc: '4 months',
			value: 36,
			soup: true
		},
		{
			place: '3rd',
			name: 'GooseGoose27',
			uuid: '4d93a367-a51a-461f-a01f-e62ff7ed4cb2',
			desc: '3 months',
			value: 27,
			soup: true
		},
		{
			place: '4th',
			name: 'AizuisMarshy',
			uuid: '22efa802-7c89-4657-a9fe-ad525f01c630',
			desc: '$18 USD Paypal',
			value: 18,
			soup: false
		}
	];
</script>

<header class="site-header">
	<div class="site-header__inner">
		<a class="logo" href="#top">no<span class="pct">%</span> invitational</a>
		<nav>
			<a href="#info">info</a>
			<a href="#vods">vods</a>
		</nav>
	</div>
</header>

<div class="page">
	<main>
		<section class="hero">
			<div class="hero-title">
				<h1>no<span class="pct">%</span><br />invitational</h1>
				<p class="tagline">a community tournament for runners who hate percentages</p>
				<div class="hero-badges">
					<span class="badge muted">#1 Concluded</span>
					<span class="badge accent">#2 Coming Soon</span>
				</div>
			</div>
			<div class="hero-embed">
				<iframe
					src="https://player.twitch.tv/?video=v2758159130&parent={dev
						? 'localhost'
						: 'ilovecatgirls.xyz'}&time=0h13m12s"
					title="Twitch Highlight for no% Invitational 1 Group Stage Seed 4"
					allowfullscreen
				>
				</iframe>
			</div>
		</section>

		<section class="section pt-0!" id="info">
			<EligibilityChecker />
			<div class="info-grid">
				<div class="info-card">
					<span class="info-label">format</span>
					<span class="info-value">TBD</span>
				</div>
				<div class="info-card">
					<span class="info-label">date</span>
					<span class="info-value">TBD</span>
				</div>
				<div class="info-card">
					<span class="info-label">entrants</span>
					<span class="info-value">TBD</span>
				</div>
				<div class="info-card">
					<span class="info-label">prize</span>
					<span class="info-value">TBD</span>
				</div>
			</div>
			<div class="description-card">
				<p>THIS SHIT NOT HAPPENING TILL LIKE LATE SUMMER 2026 I GOT TESTS DAWG</p>
			</div>
		</section>

		<div class="npi1-divider">
			<div class="npi1-divider-line"></div>
			<span class="npi1-divider-label">no<span class="pct">%</span> invitational 1</span>
			<div class="npi1-divider-line"></div>
		</div>

		<section class="section section-top" id="winners">
			<h2 class="section-title" style="text-align: center">results</h2>
			<div class="final-card">
				<div class="final-player final-player--winner">
					<img
						class="final-skin"
						src="https://nmsr.nickac.dev/fullbody/2125698b-0d79-4c4c-9969-8e39430e65fc"
						alt="junesies"
					/>
					<div class="final-player-info">
						<span class="final-crown">champion</span>
						<a
							class="final-name"
							href="https://mcsrranked.com/stats/2125698b-0d79-4c4c-9969-8e39430e65fc"
							target="_blank"
							rel="external noopener">junesies</a
						>
					</div>
				</div>
				<div class="final-score">
					<p class="final-score-text">
						<span class="final-score-num final-score-win">3</span> –
						<span class="final-score-num">1</span>
					</p>
					<span class="final-score-label">grand finals</span>
					<div class="final-stats">
						<div class="final-stat">
							<span class="final-stat-val final-stat-val--win">30:57.62</span>
							<span class="final-stat-label">avg time</span>
							<span class="final-stat-val final-stat-val--loss">31:54.13</span>
						</div>
						<div class="final-stat">
							<span class="final-stat-val final-stat-val--win">20:06.87</span>
							<span class="final-stat-label">best time</span>
							<span class="final-stat-val final-stat-val--loss">23:42.00</span>
						</div>
					</div>
				</div>
				<div class="final-player final-player--loser">
					<img
						class="final-skin"
						src="https://nmsr.nickac.dev/fullbody/0234262f-0ae0-4d6a-8f09-22f30fb5ce9e"
						alt="_MonkeyBoy_"
					/>
					<div class="final-player-info">
						<span class="final-crown final-crown--hidden">runner-up</span>
						<a
							class="final-name final-name--muted"
							href="https://mcsrranked.com/stats/0234262f-0ae0-4d6a-8f09-22f30fb5ce9e"
							target="_blank"
							rel="external noopener">_MonkeyBoy_</a
						>
					</div>
				</div>
			</div>
		</section>

		<section class="section" id="vods">
			<h2 class="section-title">group stage vods</h2>
			<GroupStageVods />
		</section>

		<section class="section" id="prize-pool">
			<h2 class="section-title">prize pool</h2>
			<div class="prize-list">
				{#each prizes as entry (entry.uuid)}
					<div
						class="prize-row prize-row--{entry.place
							.replace('th', '')
							.replace('rd', '')
							.replace('nd', '')
							.replace('st', '')}"
					>
						<span class="prize-place">{entry.place}</span>
						<a
							class="prize-player"
							href="https://mcsrranked.com/stats/{entry.uuid}"
							target="_blank"
							rel="external noopener"
						>
							<img
								class="prize-avatar"
								src="https://nmsr.nickac.dev/face/{entry.uuid}"
								alt={entry.name}
							/>
							<span class="prize-name">{entry.name}</span>
						</a>
						<span class="prize-desc">
							{entry.desc}{#if entry.soup}&nbsp;<img
									class="prize-pick"
									src={diaPick}
									alt="Diamond Pickaxe supporter"
								/>{/if}
						</span>
						<span class="prize-value">${entry.value} USD</span>
					</div>
				{/each}
				<div class="prize-row prize-row--raffle">
					<span class="prize-place prize-place--raffle">raffle</span>
					<div class="prize-player prize-player--plain">
						<span class="prize-name prize-name--muted">TBD</span>
					</div>
					<span class="prize-desc"
						>1 month&nbsp;<img
							class="prize-pick"
							src={diaPick}
							alt="Diamond Pickaxe supporter"
						/></span
					>
					<span class="prize-value">$9 USD</span>
				</div>
			</div>
		</section>

		<section class="section" id="credits">
			<h2 class="section-title" style="text-align: center">credits</h2>
			<Credits />
		</section>
	</main>

	<footer>
		<span>no% invitational</span>
		<span class="footer-muted"
			>tournament & website made with love <span style="color: var(--color-accent);">&hearts;</span
			></span
		>
		<div class="footer-gh-wrap">
			<span class="footer-commit">{__COMMIT_HASH__}</span>
			<a
				class="footer-gh"
				href="https://github.com/patttterson/haerin"
				target="_blank"
				rel="external noopener"
				aria-label="GitHub repository"
			>
				<svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" aria-hidden="true">
					<path
						d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"
					/>
				</svg>
			</a>
		</div>
	</footer>
</div>

<style>
	.page {
		width: 100%;
		max-width: 1100px;
		margin: 0 auto;
		padding: 0 1.5rem;
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	.site-header {
		position: sticky;
		top: 0;
		width: 100vw;
		margin-left: calc(50% - 50vw);
		background: var(--color-bg);
		border-bottom: 1px solid var(--color-border);
		z-index: 10;
	}
	.site-header__inner {
		display: flex;
		align-items: center;
		justify-content: space-between;
		max-width: 1100px;
		margin: 0 auto;
		padding: 1.25rem 1.5rem;
	}
	.logo {
		font-size: 1.1rem;
		font-weight: 700;
		letter-spacing: -0.02em;
		color: var(--color-text);
		text-decoration: none;
	}
	.pct {
		color: var(--color-accent);
	}
	nav {
		display: flex;
		gap: 1.5rem;
	}
	nav a {
		color: var(--color-text-muted);
		text-decoration: none;
		font-size: 0.9rem;
		transition: color 0.15s;
	}
	nav a:hover {
		color: var(--color-accent);
	}

	.hero {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 3rem;
		align-items: center;
		padding: 1.5rem 0 1rem;
	}
	@media (max-width: 640px) {
		.hero {
			grid-template-columns: 1fr;
			padding: 3rem 0 2rem;
		}
	}
	h1 {
		font-size: clamp(2.5rem, 7vw, 4.5rem);
		line-height: 1;
		margin: 0 0 1rem;
		letter-spacing: -0.03em;
	}
	.tagline {
		color: var(--color-text-muted);
		font-size: 1rem;
		margin: 0 0 1.25rem;
		line-height: 1.5;
	}
	.hero-badges {
		display: flex;
		gap: 0.5rem;
	}

	.hero-embed {
		aspect-ratio: 16 / 9;
		border-radius: 12px;
		overflow: hidden;
		border: 1px solid var(--color-border);
	}
	.hero-embed iframe {
		width: 100%;
		height: 100%;
		border: none;
		display: block;
	}

	/*
	.embed-placeholder {
		width: 100%;
		height: 100%;
		background: var(--color-surface);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0.4rem;
		color: var(--color-text-disabled);
	}
	.embed-icon { font-size: 2rem; color: var(--color-accent); opacity: 0.5; }
	.embed-placeholder p { margin: 0; font-size: 0.9rem; color: var(--color-text-muted); }
	.embed-placeholder span { font-size: 0.75rem; }
    */

	.npi1-divider {
		display: flex;
		align-items: center;
		gap: 1rem;
		padding: 0 1.5rem;
		width: 100vw;
		margin-left: calc(50% - 50vw);
	}
	.npi1-divider-line {
		flex: 1;
		height: 1px;
		background: var(--color-accent);
	}
	.npi1-divider-label {
		font-size: 0.75rem;
		font-weight: 700;
		text-transform: uppercase;
		letter-spacing: 0.12em;
		color: var(--color-text);
		white-space: nowrap;
	}
	.npi1-divider-label .pct {
		color: var(--color-accent);
	}

	.section {
		padding: 3rem 0;
		border-top: 1px solid var(--color-border);
		scroll-margin-top: 4rem;
	}
	.section-top {
		border-top: 0;
		scroll-margin-top: 5rem;
	}
	.section-title {
		font-size: 0.75rem;
		text-transform: uppercase;
		letter-spacing: 0.12em;
		color: var(--color-accent);
		margin: 0 0 1.5rem;
	}

	.prize-list {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}
	.prize-row {
		display: grid;
		grid-template-columns: 3rem 1fr 1fr auto;
		align-items: center;
		gap: 1rem;
		background: var(--color-surface);
		border: 1px solid var(--color-border);
		border-radius: 10px;
		padding: 0.75rem 1.25rem;
		transition: border-color 0.15s;
	}
	.prize-row--1 {
		border-left: 3px solid #f5a623;
	}
	.prize-row--2 {
		border-left: 3px solid #a0a0b0;
	}
	.prize-row--3 {
		border-left: 3px solid #cd7f32;
	}
	.prize-row--4 {
		border-left: 3px solid var(--color-border);
	}
	.prize-row--raffle {
		border-left: 3px solid var(--color-border);
		opacity: 0.7;
	}
	.prize-place {
		font-size: 0.75rem;
		font-weight: 700;
		text-transform: uppercase;
		letter-spacing: 0.06em;
		color: var(--color-text-disabled);
	}
	.prize-row--1 .prize-place {
		color: #f5a623;
	}
	.prize-row--2 .prize-place {
		color: #a0a0b0;
	}
	.prize-row--3 .prize-place {
		color: #cd7f32;
	}
	.prize-place--raffle {
		letter-spacing: 0.04em;
	}
	.prize-player {
		display: flex;
		align-items: center;
		gap: 0.6rem;
		text-decoration: none;
	}
	.prize-player--plain {
		cursor: default;
	}
	.prize-avatar {
		width: 28px;
		height: 28px;
		image-rendering: pixelated;
		flex-shrink: 0;
	}
	.prize-name {
		font-size: 0.9rem;
		font-weight: 600;
		color: var(--color-text);
	}
	.prize-player:not(.prize-player--plain):hover .prize-name {
		color: var(--color-accent);
	}
	.prize-name--muted {
		color: var(--color-text-muted);
		font-weight: 400;
	}
	.prize-pick {
		display: inline;
		width: 1.6em;
		height: 1.6em;
		margin-left: 0.2em;
		vertical-align: middle;
		image-rendering: pixelated;
	}
	.prize-desc {
		font-size: 0.8rem;
		color: var(--color-text-muted);
	}
	.prize-value {
		font-size: 0.85rem;
		font-weight: 700;
		color: var(--color-text-disabled);
		font-variant-numeric: tabular-nums;
		text-align: right;
	}
	@media (max-width: 600px) {
		.prize-row {
			grid-template-columns: 3rem 1fr auto;
		}
		.prize-desc {
			display: none;
		}
	}

	.info-grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 0.75rem;
		margin-bottom: 1rem;
	}
	.info-card {
		background: var(--color-surface);
		border: 1px solid var(--color-border);
		border-radius: 10px;
		padding: 1rem 1.25rem;
		display: flex;
		flex-direction: column;
		gap: 0.3rem;
	}
	.info-label {
		font-size: 0.7rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--color-text-disabled);
	}
	.info-value {
		font-size: 1.1rem;
		font-weight: 600;
		color: var(--color-text);
	}
	.description-card {
		background: var(--color-surface);
		border: 1px solid var(--color-border);
		border-radius: 10px;
		padding: 1.25rem;
		color: var(--color-text-muted);
		font-size: 0.95rem;
		line-height: 1.6;
	}
	.description-card p {
		margin: 0;
	}

	.badge {
		border-radius: 999px;
		padding: 0.2rem 0.75rem;
		font-size: 0.75rem;
		font-weight: 600;
	}
	.badge.accent {
		background: color-mix(in srgb, var(--color-accent) 20%, transparent);
		color: var(--color-accent);
	}
	.badge.muted {
		background: color-mix(in srgb, var(--color-text-muted) 20%, transparent);
		color: var(--color-text-muted);
	}

	.final-card {
		display: grid;
		grid-template-columns: 1fr auto 1fr;
		align-items: end;
		gap: 1.5rem;
	}
	.final-player {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.75rem;
	}
	.final-player--loser {
		opacity: 0.45;
		filter: grayscale(0.4);
	}
	.final-player--loser .final-skin {
		transform: scaleX(-1);
	}
	.final-skin {
		height: 180px;
		width: auto;
		image-rendering: pixelated;
	}
	.final-player-info {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.2rem;
	}
	.final-crown {
		font-size: 0.65rem;
		text-transform: uppercase;
		letter-spacing: 0.14em;
		color: #f5a623;
		font-weight: 700;
	}
	.final-crown--hidden {
		visibility: hidden;
	}
	.final-name {
		font-size: 1.1rem;
		font-weight: 700;
		color: var(--color-text);
		text-decoration: none;
		letter-spacing: -0.01em;
	}
	.final-name:hover {
		color: var(--color-accent);
	}
	.final-name--muted {
		color: var(--color-text-muted);
	}
	.final-score {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.4rem;
		align-self: center;
	}
	.final-score-text {
		font-size: 4rem;
		font-weight: 700;
		letter-spacing: -0.04em;
		color: var(--color-text);
		line-height: 1;
		margin: 0;
		text-align: center;
	}
	.final-score-win {
		color: #f5a623;
	}
	.final-stats {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		margin-top: 0.75rem;
		width: 100%;
	}
	.final-stat {
		display: grid;
		grid-template-columns: 1fr auto 1fr;
		align-items: center;
		gap: 0.5rem;
	}
	.final-stat-label {
		font-size: 0.6rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		color: var(--color-text-disabled);
		text-align: center;
		white-space: nowrap;
	}
	.final-stat-val {
		font-size: 0.8rem;
		font-weight: 600;
	}
	.final-stat-val--win {
		color: var(--color-text);
		text-align: right;
	}
	.final-stat-val--loss {
		color: var(--color-text-muted);
		text-align: left;
	}
	.final-score-num {
		display: inline-block;
		width: 1.2ch;
		text-align: center;
	}
	.final-score-label {
		font-size: 0.6rem;
		text-transform: uppercase;
		letter-spacing: 0.12em;
		color: var(--color-text-disabled);
	}
	@media (max-width: 500px) {
		.final-card {
			grid-template-columns: 1fr;
			justify-items: center;
		}
		.final-player--loser {
			grid-row: 3;
		}
		.final-score {
			grid-row: 2;
			padding: 0;
		}
	}

	footer {
		margin-top: auto;
		padding: 2rem 0;
		border-top: 1px solid var(--color-border);
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 0.8rem;
		color: var(--color-text-muted);
	}
	footer > span:first-child {
		flex: 1;
	}
	.footer-muted {
		color: var(--color-text-disabled);
	}
	.footer-gh-wrap {
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: flex-end;
		gap: 0.4rem;
	}
	.footer-commit {
		font-size: 0.7rem;
		font-family: monospace;
		color: var(--color-text-disabled);
	}
	.footer-gh {
		color: var(--color-text-disabled);
		display: flex;
		align-items: center;
		transition: color 0.15s;
	}
	.footer-gh:hover {
		color: var(--color-accent);
	}
</style>
