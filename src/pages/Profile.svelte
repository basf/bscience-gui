{#if $user}
	<Panel>
		<div slot="header" class="text-center">
			<Avatar size="xl" bg="#eee" name={$user.firstName + ' ' + $user.lastName} caption />
		</div>
		<Tabs slot="nav" items={profile} bind:active block />
		<div slot="body" class="mt-2">
			<Tile>
				<span slot="title">E-mail: <a href="mailto:{$user.email}">{$user.email}</a></span>
			</Tile>
		</div>

		<svelte:fragment slot="footer">
			<Button on:click={doLogout} variant="primary" block>Log out</Button>
		</svelte:fragment>
	</Panel>
{/if}

<script lang="ts" context="module">
	import { Avatar, Button, Panel, Tabs, Tile, toast } from 'svelte-spectre';

	import user, { userAsync } from '@/stores/user';

	import { logout } from '@/services/api';
</script>

<script lang="ts">
	let profile = [{ title: 'Profile' }],
		active = 1;

	async function doLogout() {
		await logout();
		$userAsync = null;
		toast.warning({ msg: 'You are logged out', timeout: 4000, pos: 'top_right' });
	}
</script>

<style>
	/* tmp theme fix */
	@media (prefers-color-scheme: dark) {
		:global(.spectre .avatar figcaption) {
			color: #f2f2f2 !important;
		}
	}
	@media (prefers-color-scheme: light) {
		:global(.spectre .avatar figcaption) {
			color: #3b4351 !important;
		}
	}
	:global([color-scheme='dark'] .spectre .avatar figcaption) {
		color: #f2f2f2 !important;
	}
	:global([color-scheme='light'] .spectre .avatar figcaption) {
		color: #3b4351 !important;
	}
	.text-center :global(figcaption) {
		width: max-content;
	}
</style>
