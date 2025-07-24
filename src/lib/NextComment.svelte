<script>
	import Chat from './Chat.svelte';

	let currentIndex = 1;
	export let comments = [];

	$: size1 = 6 * (currentIndex - 1);
	$: size2 = 6 * currentIndex;
	$: totalPages = Math.ceil(comments.length / 6) || 1;

	function nextContent() {
		if (currentIndex < totalPages) {
			currentIndex += 1;
		}
	}

	function prevContent() {
		if (currentIndex > 1) {
			currentIndex -= 1;
		}
	}

	function goToPage(page) {
		currentIndex = page;
	}

	$: pageNumbers = (() => {
		const maxButtons = 5;
		if (totalPages <= maxButtons) {
			return Array.from({ length: totalPages }, (_, i) => i + 1);
		}

		const pages = [];
		const start = Math.max(2, currentIndex - 2);
		const end = Math.min(totalPages - 1, start + 2);

		pages.push(1);
		if (start > 2) {
			pages.push('...');
		}

		for (let i = start; i <= end && pages.length < maxButtons; i++) {
			pages.push(i);
		}

		if (end < totalPages - 1) {
			pages.push('...');
		}

		if (totalPages > 1 && pages.length < maxButtons) {
			pages.push(totalPages);
		}

		return pages;
	})();
</script>

<div class="content-container">
	<div class="chats">
		{#if comments.length === 0}
			<p>No comments available</p>
		{:else}
			{#each comments as comment, index}
				{#if index >= size1 && index < size2}
					<Chat {comment} />
				{/if}
			{/each}
		{/if}
	</div>

	<div class="nav-buttons">
		<button on:click={prevContent} disabled={currentIndex === 1}>Previous</button>

		{#each pageNumbers as page}
			{#if page === '...'}
				<span class="ellipsis">...</span>
			{:else}
				<button
					on:click={() => goToPage(page)}
					class:active={currentIndex === page}
					disabled={currentIndex === page}
					aria-label={`Go to page ${page}`}>{page}</button
				>
			{/if}
		{/each}

		<button on:click={nextContent} disabled={currentIndex >= totalPages}>Next</button>
	</div>
</div>

<style>
	.content-container {
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
		width: 100%;
	}
	.nav-buttons {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 5px;
	}

	.chats {
		width: 100%;
		height: 250px;
		overflow: auto;
		scrollbar-width: none;
		padding: 4px;
	}

	button {
		color: #3b5998;
		font-weight: bold;
		margin: 0 5px;
		font-size: 14px;
		cursor: pointer;
		border: none;
	}

	.nav-buttons button:not(:first-child):not(:last-child) {
		padding: 8px 0px;
	}

	.active {
		font-weight: bold;
		text-decoration-line: underline;
	}

	button:active {
		background-color: transparent !important;
	}

	button:disabled {
		cursor: not-allowed;
	}
</style>