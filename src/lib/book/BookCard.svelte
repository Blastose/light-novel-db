<script lang="ts">
	import { supabaseClient } from '$lib/db';
	import type Book from '$lib/models/book';
	import { onMount } from 'svelte';

	export let book: Book;

	let coverUrl: string | null;
	const getCover = () => {
		let { publicURL } = supabaseClient.storage
			.from('cover-images')
			.getPublicUrl(`${book.cover_image_file_name}.jpg`);

		coverUrl = publicURL;
	};

	onMount(getCover);
</script>

<div
	class="bg-[#e4e7ee] p-2 rounded-sm grid grid-cols-[1fr_75%] md:grid-cols-[150px_1fr] gap-x-2 shadow-sm"
>
	<a href="/book/{book.id}" sveltekit:prefetch>
		<img class="shadow-sm rounded-sm" src={coverUrl} alt="Cover image for {book.title}" />
	</a>
	<div class="flex flex-col gap-2 ">
		<a href="/book/{book.id}" sveltekit:prefetch>
			<span class="font-bold text-xl">{book.title}</span>
		</a>
		<div class="relative blur-text overflow-hidden h-full">
			<p class="min-h-min max-h-[100px] sm:max-h-[200px] text-sm text-[#263147]">
				{@html book.description}
			</p>
		</div>
	</div>
</div>

<style>
	.blur-text:after {
		content: '';
		width: 100%;
		height: 1em;
		position: absolute;
		left: 0;
		bottom: 0;
		background: linear-gradient(transparent, #e4e7ee);
	}
</style>
