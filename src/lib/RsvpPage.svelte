<script>
	import { onMount } from 'svelte';
	import RSVPForm from './RSVPForm.svelte';
	import WishesForm from './WishesForm.svelte';
	import NextComment from './NextComment.svelte';

	let comments = [];
	let responseMessage = '';

	async function fetchComments() {
		try {
			const response = await fetch('http://localhost:3000/comments');
			if (response.ok) {
				comments = await response.json();
			} else {
				responseMessage = 'Error saat mengambil komentar dari database';
			}
		} catch (error) {
			responseMessage = 'Error saat mengambil komentar: ' + error.message;
		}
	}

	function handleSubmit(event) {
		responseMessage = event.detail.message;
	}

	onMount(fetchComments);
</script>

<section>
	<div class="main-container overflow-hidden">
		<div class="frame-rsvp-wishes">
			<RSVPForm {responseMessage} on:submit={handleSubmit} />
			<WishesForm {comments} {responseMessage} on:submit={handleSubmit} />
		</div>
	</div>
</section>

<style>
	.main-container {
		width: 100%;
		padding: 50px 15px;
	}

	.frame-rsvp-wishes {
		width: 100%;
		padding: 30px 15px;
		background-color: #051d30;
		border-radius: 20px;
	}
</style>