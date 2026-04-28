<script lang="ts">
	import mendingHeart from '$lib/assets/mending-heart.png';
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
</script>

<div class="eligibility-panel -mt-1! -mb-2!" class:is-idle={eligibilityState === 'idle'}>
	<div class="eligibility-left">
		<p class="eligibility-sign">check eligibility</p>
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

		<div class="requirements">
			<span class="requirements-label">requirement</span>
			less than 15% forfeit rate in the current season
		</div>
	</div>
	<div class="eligibility-divider"></div>
	<div class="eligibility-right">
		{#if eligibilityState === 'idle'}
			<span class="eligibility-idle">←</span>
		{:else if eligibilityState === 'loading'}
			<span class="eligibility-idle">...</span>
		{:else if ignInput.toLowerCase() === 'submissivecatgir'}
			<p class="eligibility-verdict eligibility-verdict--yes">hell yeah twin <img src={mendingHeart} alt="🩹" class="inline-emoji" /></p>
			<span class="eligibility-ff">-55% ff rate this season</span>
			<span class="eligibility-note">i dont make the rules (i do)</span>
		{:else if eligibilityState === 'eligible'}
			{#if seasonalFf === 0}
				<p class="eligibility-verdict eligibility-verdict--yes eligibility-verdict--top">
					W MENTALL
				</p>
			{:else}
				<p class="eligibility-verdict eligibility-verdict--yes">you're eligible!</p>
			{/if}
			<span class="eligibility-ff">{seasonalFf?.toFixed(2)}% ff rate this season</span>
			{#if seasonalFf === 0}
				<p class="eligibility-note">(you qualify)</p>
			{/if}
		{:else if eligibilityState === 'ineligible'}
			<p class="eligibility-verdict eligibility-verdict--no">not eligible for NPI.</p>
			<span class="eligibility-ff">{seasonalFf?.toFixed(2)}% ff rate this season</span>
			<p class="eligibility-note">do better next season &hearts;</p>
		{:else if eligibilityState === 'error'}
			<p class="eligibility-verdict eligibility-verdict--error">{eligibilityMessage}</p>
		{/if}
	</div>
</div>
<div class="eligibility-hr"></div>

<style>
	.eligibility-panel {
		display: grid;
		grid-template-columns: 1fr auto 1fr;
		align-items: center;
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
		align-self: center;
		height: 80%;
		background: var(--color-border);
	}
	.eligibility-right {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0.35rem;
		padding: 1.25rem 0;
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
	.inline-emoji {
		display: inline;
		width: 1.2em;
		height: 1.2em;
		vertical-align: -0.2em;
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
	.eligibility-hr {
		height: 1px;
		background: var(--color-border);
		margin-bottom: 0.75rem;
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
		.eligibility-panel.is-idle .eligibility-divider,
		.eligibility-panel.is-idle .eligibility-right {
			display: none;
		}
	}

	.requirements {
		display: flex;
		align-items: baseline;
		gap: 0.75rem;
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
</style>
