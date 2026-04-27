<script lang="ts">
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

	function initials(name: string) {
		return name.slice(0, 2).toUpperCase();
	}

	function avatarColor(name: string) {
		let hash = 0;
		for (let i = 0; i < name.length; i++) hash = name.charCodeAt(i) + ((hash << 5) - hash);
		return palette[Math.abs(hash) % palette.length];
	}
</script>

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

<style>
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
</style>
