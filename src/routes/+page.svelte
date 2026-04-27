<script lang="ts">
	import { dev } from '$app/environment';
	import thumb2758159131 from '$lib/assets/thumbnails/thumb2758159131.jpg';
	import thumb2758159128 from '$lib/assets/thumbnails/thumb2758159128.jpg';
	import thumb2758159130 from '$lib/assets/thumbnails/thumb2758159130.jpg';
	import thumb2758159129 from '$lib/assets/thumbnails/thumb2758159129.jpg';
	import diaPick from '$lib/assets/dia_pick.png';

	interface CreditPerson {
		name: string;
		note?: string;
		uuid?: string;
	}
	interface CreditGroup {
		role: string;
		people: CreditPerson[];
		layout?: 'grid' | 'bubble-grid';
	}

	const credits: CreditGroup[] = [
		{
			role: 'organizers',
			layout: 'grid',
			people: [
				{ name: 'patty', note: 'organizer', uuid: '652a4e7f-163d-41d5-8fae-cb47f59fb12c' },
				{ name: 'sen', note: 'basically everything', uuid: '7d5a64b2-8967-4db6-9b10-18441c860e22' },
				{ name: 'classic', note: 'co-host, manager', uuid: '8e4e0d33-1ba4-4b7f-ba66-4fee8a2916f9' }
			]
		},
		{
			role: 'commentators',
			people: [
				{ name: 'cylan', note: 'stream manager', uuid: '06935965-1d4a-42da-b6d2-6915b4583367' },
				{ name: 'sarah', uuid: 'f7e27b19-2add-45da-a24c-1e12d1d334de' },
				{ name: 'julie', uuid: 'aca29442-2ad9-4b50-9aef-a1f23d8b51eb' },
				{ name: 'skye', uuid: '2481d626-8479-48c1-aad7-41c0f380143d' }
			]
		},
		{
			role: 'players',
			layout: 'bubble-grid',
			people: [
				{ name: 'junesies', uuid: '2125698b-0d79-4c4c-9969-8e39430e65fc' },
				{ name: '_MonkeyBoy_', uuid: '0234262f-0ae0-4d6a-8f09-22f30fb5ce9e' },
				{ name: 'GooseGoose27', uuid: '4d93a367-a51a-461f-a01f-e62ff7ed4cb2' },
				{ name: 'AizuisMarshy', uuid: '22efa802-7c89-4657-a9fe-ad525f01c630' },
				{ name: 'B8zer', uuid: '8720dade-8cd9-4247-ae06-a90d3578b8cf' },
				{ name: 'Apolytus', uuid: '97308061-4b05-4d7a-9690-fd83f505f0dd' },
				{ name: 'RTCRhino', uuid: 'b60bee7a-f5d2-494d-99de-dfd864de2bd4' },
				{ name: 'greatsr', uuid: 'e20b93dd-6b78-4644-816c-40b8c115464e' },
				{ name: 'YamiDoll_exe', uuid: '8f114abd-6434-4c1d-a765-761ec9fcf2cc' },
				{ name: 'Cyphered', uuid: '53b4e33c-6d9b-4b21-af17-a4c4e262e7bf' },
				{ name: 'Rtok', uuid: 'b1e623e7-c941-452c-bffd-24acb928525b' },
				{ name: 'EmiPllle', uuid: '65f83033-5592-4cde-849e-0cc30267a67b' },
				{ name: 'CroissantGamer', uuid: '9d0b790b-6d89-47ee-a9c4-1e7866a538e1' },
				{ name: 'Silviore', uuid: '7a9eab13-3401-4126-9d5a-9a2732a00c79' },
				{ name: 'r_ddragon', uuid: '188ea5ef-29f1-4c05-841a-5c94e5e51184' },
				{ name: 'Sephoia', uuid: '0b531db7-9beb-4fe7-a327-20f4a2ab5489' },
				{ name: 'Bright_NVPY', uuid: '1c78d6b3-c763-4f72-9813-e2a61f833d43' },
				{ name: 'Tiipped', uuid: 'b6d207b9-99c9-41c0-b6a9-0ceaef74c7e9' }
			]
		}
	];

	function initials(name: string) {
		return name.slice(0, 2).toUpperCase();
	}

	const palette = [
		'#f72585',
		'#7209b7',
		'#3a0ca3',
		'#4361ee',
		'#4cc9f0',
		'#2dd4a0',
		'#f5a623',
		'#ff5c6c'
	];
	function avatarColor(name: string) {
		let hash = 0;
		for (let i = 0; i < name.length; i++) hash = name.charCodeAt(i) + ((hash << 5) - hash);
		return palette[Math.abs(hash) % palette.length];
	}

	interface Vod {
		twitchVideoId: number | null;
		thumbnailUrl: string | null;
		gameNumber: number;
		matchId: number | null;
	}

	let ignInput = '';
	let eligibilityState: 'idle' | 'loading' | 'eligible' | 'ineligible' | 'error' = 'idle';
	let eligibilityMessage = '';
	let seasonalFf: number | null = null;

	async function checkEligibility() {
		const ign = ignInput.trim();
		if (!ign) return;
		eligibilityState = 'loading';
		seasonalFf = null;
		try {
			const resp = await fetch(`https://api.mcsrranked.com/users/${encodeURIComponent(ign)}`);
			if (!resp.ok) {
				eligibilityState = 'error';
				eligibilityMessage = `could not find player "${ign}"`;
				return;
			}
			const data = await resp.json();
			const stats = data.data.statistics;
			const ff = (stats.season.forfeits.ranked / stats.season.playedMatches.ranked) * 100;
			seasonalFf = ff;
			if (ff <= 15) {
				eligibilityState = 'eligible';
			} else {
				eligibilityState = 'ineligible';
			}
		} catch {
			eligibilityState = 'error';
			eligibilityMessage = 'something went wrong, try again';
		}
	}

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

	const groupStageVods: Vod[] = [
		{ twitchVideoId: null, thumbnailUrl: null, gameNumber: 1, matchId: 9350856 },
		{ twitchVideoId: 2758159131, thumbnailUrl: thumb2758159131, gameNumber: 2, matchId: 9351763 },
		{ twitchVideoId: 2758159128, thumbnailUrl: thumb2758159128, gameNumber: 3, matchId: 9353622 },
		{ twitchVideoId: 2758159130, thumbnailUrl: thumb2758159130, gameNumber: 4, matchId: 9354952 },
		{ twitchVideoId: 2758159129, thumbnailUrl: thumb2758159129, gameNumber: 5, matchId: null }
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

		<section class="section section-top !pt-0" id="info">
			<h2 class="section-title">info</h2>
			<div class="eligibility-panel">
				<div class="eligibility-left">
					<p class="eligibility-sign">check<br />eligibility</p>
					<form class="eligibility-form" on:submit|preventDefault={checkEligibility}>
						<input
							class="ign-input"
							type="text"
							placeholder="your IGN..."
							bind:value={ignInput}
							autocomplete="off"
							spellcheck="false"
						/>
						<button class="ign-btn" type="submit" disabled={eligibilityState === 'loading'}>
							{eligibilityState === 'loading' ? '...' : 'check'}
						</button>
					</form>
				</div>
				<div class="eligibility-divider"></div>
				<div class="eligibility-right">
					{#if eligibilityState === 'idle'}
						<span class="eligibility-idle">—</span>
					{:else if eligibilityState === 'loading'}
						<span class="eligibility-idle">...</span>
					{:else if ignInput.toLowerCase() === 'submissivecatgir'}
						<p class="eligibility-verdict eligibility-verdict--yes">for sure twin</p>
						<span class="eligibility-ff">-55% ff rate this season</span>
						<span class="eligibility-note">what did you think was gonna happen?</span>
					{:else if eligibilityState === 'eligible'}
						{#if seasonalFf === 0}
							<p class="eligibility-verdict eligibility-verdict--yes eligibility-verdict--top">
								W MENTALL
							</p>
						{:else}
							<p class="eligibility-verdict eligibility-verdict--yes">you're eligible!</p>
						{/if}
						<span class="eligibility-ff">{seasonalFf?.toFixed(2)}% ff rate this season</span>
					{:else if eligibilityState === 'ineligible'}
						<p class="eligibility-verdict eligibility-verdict--no">not eligible for NPI.</p>
						<span class="eligibility-ff">{seasonalFf?.toFixed(2)}% ff rate this season</span>
					{:else if eligibilityState === 'error'}
						<p class="eligibility-verdict eligibility-verdict--error">{eligibilityMessage}</p>
					{/if}
				</div>
			</div>
			<div class="requirements">
				<span class="requirements-label">requirement</span>
				less than 15% forfeit rate in the current season
			</div>
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
			<div class="vod-grid">
				{#each groupStageVods as vod (vod.gameNumber)}
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
		</section>

		<section class="section" id="prize-pool">
			<h2 class="section-title">prize pool</h2>
			<div class="prize-list">
				{#each prizes as entry}
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
			<div class="credits-groups">
				{#each credits as group (group.role)}
					<div class="credits-group">
						<span class="credits-role">{group.role}</span>
						{#if group.layout === 'grid'}
							<div class="credits-grid">
								{#each group.people as person (person.name)}
									<div class="credits-grid-card">
										{#if person.uuid}
											<a
												class="credit-link"
												href="https://mcsrranked.com/stats/{person.uuid}"
												target="_blank"
												rel="external noopener"
											>
												<img
													class="avatar"
													src="https://nmsr.nickac.dev/face/{person.uuid}"
													alt={person.name}
												/>
												<span class="credit-name">{person.name}</span>
											</a>
										{:else}
											<div class="avatar" style="background: {avatarColor(person.name)}">
												{initials(person.name)}
											</div>
											<span class="credit-name">{person.name}</span>
										{/if}
										{#if person.note}<span class="credit-note">{person.note}</span>{/if}
									</div>
								{/each}
							</div>
						{:else if group.layout === 'bubble-grid'}
							<div class="credits-bubble-grid">
								{#each group.people as person (person.name)}
									<div class="credit-person">
										{#if person.uuid}
											<a
												class="credit-link"
												href="https://mcsrranked.com/stats/{person.uuid}"
												target="_blank"
												rel="external noopener"
											>
												<img
													class="avatar"
													src="https://nmsr.nickac.dev/face/{person.uuid}"
													alt={person.name}
												/>
												<span class="credit-name">{person.name}</span>
											</a>
										{:else}
											<div class="avatar" style="background: {avatarColor(person.name)}">
												{initials(person.name)}
											</div>
											<span class="credit-name">{person.name}</span>
										{/if}
										{#if person.note}<span class="credit-note">{person.note}</span>{/if}
									</div>
								{/each}
							</div>
						{:else}
							<div class="credits-cloud">
								{#each group.people as person (person.name)}
									<div class="credit-person">
										{#if person.uuid}
											<a
												class="credit-link"
												href="https://mcsrranked.com/stats/{person.uuid}"
												target="_blank"
												rel="external noopener"
											>
												<img
													class="avatar"
													src="https://nmsr.nickac.dev/face/{person.uuid}"
													alt={person.name}
												/>
												<span class="credit-name">{person.name}</span>
											</a>
										{:else}
											<div class="avatar" style="background: {avatarColor(person.name)}">
												{initials(person.name)}
											</div>
											<span class="credit-name">{person.name}</span>
										{/if}
										{#if person.note}<span class="credit-note">{person.note}</span>{/if}
									</div>
								{/each}
							</div>
						{/if}
					</div>
				{/each}
			</div>
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
		padding: 5rem 0 4rem;
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
		padding: 2rem 1.5rem 0;
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
	}
	.section-title {
		font-size: 0.75rem;
		text-transform: uppercase;
		letter-spacing: 0.12em;
		color: var(--color-accent);
		margin: 0 0 1.5rem;
	}

	.eligibility-panel {
		display: grid;
		grid-template-columns: 1fr auto 1fr;
		align-items: center;
		margin-bottom: 0.75rem;
		overflow: hidden;
	}
	.eligibility-left {
		display: flex;
		flex-direction: column;
		gap: 1rem;
		padding: 1.25rem;
		padding-left: 0;
	}
	.eligibility-sign {
		margin: 0;
		font-size: 1.6rem;
		font-weight: 800;
		letter-spacing: -0.03em;
		line-height: 1.1;
		color: var(--color-accent);
		text-transform: lowercase;
	}
	.eligibility-form {
		display: flex;
		gap: 0.5rem;
	}
	.ign-input {
		flex: 1;
		min-width: 0;
		background: var(--color-bg);
		border: 1px solid var(--color-border);
		border-radius: 6px;
		padding: 0.5rem 0.75rem;
		font-size: 0.9rem;
		color: var(--color-text);
		font-family: inherit;
		outline: none;
		transition: border-color 0.15s;
	}
	.ign-input:focus {
		border-color: var(--color-accent);
	}
	.ign-btn {
		background: color-mix(in srgb, var(--color-accent) 15%, transparent);
		border: 1px solid var(--color-accent);
		border-radius: 6px;
		padding: 0.5rem 1rem;
		font-size: 0.85rem;
		font-weight: 600;
		color: var(--color-accent);
		font-family: inherit;
		cursor: pointer;
		transition: background 0.15s;
		white-space: nowrap;
		min-width: 4.75rem;
	}
	.ign-btn:hover:not(:disabled) {
		background: color-mix(in srgb, var(--color-accent) 28%, transparent);
	}
	.ign-btn:disabled {
		opacity: 0.5;
		cursor: default;
	}
	.eligibility-divider {
		width: 1px;
		align-self: stretch;
		background: var(--color-border);
	}
	.eligibility-right {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0.35rem;
		padding: 1.25rem;
		padding-right: 0;
		min-height: 110px;
		text-align: center;
	}
	.eligibility-idle {
		font-size: 1.5rem;
		color: var(--color-text-disabled);
	}
	.eligibility-verdict {
		margin: 0;
		font-size: 1.25rem;
		font-weight: 800;
		letter-spacing: -0.02em;
		line-height: 1.2;
	}
	.eligibility-verdict--yes {
		color: #2dd4a0;
	}
	.eligibility-verdict--no {
		color: #ff5c6c;
	}
	.eligibility-verdict--error {
		font-size: 0.9rem;
		font-weight: 400;
		color: var(--color-text-disabled);
	}
	.eligibility-ff {
		font-size: 0.75rem;
		color: var(--color-text-disabled);
		letter-spacing: 0.04em;
	}
	@media (max-width: 640px) {
		.eligibility-panel {
			grid-template-columns: 1fr;
		}
		.eligibility-divider {
			width: auto;
			height: 1px;
			align-self: auto;
		}
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

	.requirements {
		display: flex;
		align-items: baseline;
		gap: 0.75rem;
		margin-bottom: 0.75rem;
		padding: 0 0.25rem;
		font-size: 0.85rem;
		color: var(--color-text-muted);
	}
	.requirements-label {
		font-size: 0.7rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--color-text-disabled);
		white-space: nowrap;
		flex-shrink: 0;
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
		background: rgba(0, 0, 0, 0.45);
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
	.vod-timeline:disabled {
		color: var(--color-text-disabled);
		pointer-events: none;
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

	.credits-groups {
		display: flex;
		flex-direction: column;
		gap: 3rem;
		align-items: center;
	}
	.credits-group {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1.25rem;
		width: 100%;
	}
	.credits-role {
		font-size: 0.8rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		color: var(--color-text-disabled);
	}
	.credits-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
		gap: 1rem;
		width: 100%;
	}
	.credits-grid-card {
		background: var(--color-surface);
		border: 1px solid var(--color-border);
		border-radius: 12px;
		padding: 1.5rem 1rem;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.6rem;
	}
	.credits-grid-card .avatar {
		width: 64px;
		height: 64px;
		font-size: 1.1rem;
	}
	.credits-grid-card .credit-name {
		font-size: 0.9rem;
		font-weight: 600;
		color: var(--color-text);
		white-space: normal;
	}
	.credits-grid-card .credit-note {
		font-size: 0.75rem;
	}

	.credit-link {
		display: contents;
		text-decoration: none;
	}
	.credit-link .credit-name {
		color: var(--color-text-muted);
	}

	.credits-bubble-grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(96px, 1fr));
		justify-items: center;
		gap: 1.75rem 1rem;
		width: 100%;
	}

	.credits-cloud {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		gap: 1.75rem 2.25rem;
		padding: 0.5rem 0 1rem;
	}
	.credit-person {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.6rem;
		width: auto;
		min-width: 72px;
		position: relative;
	}
	.avatar {
		width: 72px;
		height: 72px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 1rem;
		font-weight: 700;
		color: #fff;
		flex-shrink: 0;
	}
	img.avatar {
		border-radius: 0;
		object-fit: cover;
	}
	.credit-name {
		font-size: 0.65rem;
		color: var(--color-text-muted);
		text-align: center;
		white-space: nowrap;
		line-height: 1.2;
	}
	.credit-note {
		font-size: 0.75rem;
		color: var(--color-text-disabled);
		text-align: center;
		line-height: 1.2;
	}
	.credits-cloud .credit-note {
		position: absolute;
		top: 105%;
		left: 50%;
		transform: translateX(-50%);
		white-space: nowrap;
		pointer-events: none;
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
