<script lang="ts">
	import '../app.css';
	import Header from '$lib/header/Header.svelte';
	import Sidebar from '$lib/sidebar/Sidebar.svelte';
	import { windowWidth } from '$lib/stores/windowWidthStore';
	import { onMount } from 'svelte';
	import { sidebarOpen } from '$lib/stores/sidebarStore';
	import { session } from '$app/stores';
	import { supabaseClient } from '$lib/db';
	import { SupaAuthHelper } from '@supabase/auth-helpers-svelte';

	let initialLoad = true;

	onMount(async () => {
		if ($windowWidth <= 1000) {
			sidebarOpen.set(false);
		}
		initialLoad = false;
	});
</script>

<svelte:head><title>Light Novel DB</title></svelte:head>
<svelte:window bind:innerWidth={$windowWidth} />

<SupaAuthHelper {supabaseClient} {session} autoRefreshToken={true}>
	{#if initialLoad}
		<div class="flex bg-[#fafafa]" />
	{:else}
		<div class="flex">
			<Sidebar />

			<!-- Hack to make screen full on smaller widths -->
			<!-- Normally done on sidebar, but sidebar is removed from flow on small screens -->
			{#if $windowWidth <= 1000}
				<div class="h-screen" />
			{/if}

			<div class="flex flex-col flex-grow bg-[#fafafa]">
				<Header />

				<div class="flex-grow">
					<main class="container mx-auto px-8 py-4 duration-150">
						<slot />
					</main>
				</div>
			</div>
		</div>
	{/if}
</SupaAuthHelper>
