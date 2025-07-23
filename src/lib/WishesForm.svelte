<script>
	import { createEventDispatcher } from 'svelte';
	import Chat from './Chat.svelte';
	import NextComment from './NextComment.svelte';

	export let responseMessage = '';
	export let comments = [];

	let name = '';
	let message = '';

	const dispatch = createEventDispatcher();

	// Urutkan comments dari yang terbaru
	$: sortedComments = [...comments].sort((a, b) => new Date(b.created_at) - new Date(a.created_at));

	async function handleSubmit() {
		try {
			const response = await fetch('http://localhost:3000/kirim', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ name, message })
			});

			const result = await response.json();
			responseMessage = result.message;

			if (response.ok) {
				// Tambahkan komentar baru ke comments
				comments = [...comments, { name, message, created_at: new Date().toISOString() }];
				name = '';
				message = '';
				dispatch('submit', { message: result.message });
			}
		} catch (error) {
			responseMessage = 'Error saat mengirim formulir: ' + error.message;
			dispatch('submit', { message: responseMessage });
		}
	}
</script>

<div class="frame-wishes">
	<div class="title">
		<h2>Wishes</h2>
		<p>Berikan Ucapan & Do'a Terbaik Untuk kami</p>
	</div>

	<div class="wishes">
		<div class="container">
			<form on:submit|preventDefault={handleSubmit}>
				<div class="form-group">
					<label for="name">Nama:</label>
					<input type="text" id="name" bind:value={name} required />
				</div>
				<div class="form-group">
					<label for="message">Pesan:</label>
					<textarea id="message" bind:value={message} required></textarea>
				</div>
				<button type="submit">Kirim</button>
			</form>
		</div>

		<NextComment comments={sortedComments}></NextComment>
	</div>
</div>

<style>
	.wishes {
		color: rgb(0, 0, 0);
		display: flex;
		flex-direction: column;
		align-self: center;
		background-color: #eff2f6;
		border-radius: 10px;
	}

	.title {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
		margin-bottom: 30px;
	}

	.title h2 {
		font-family: 'wasted-vindey', sans-serif;
		font-size: 30px;
		color: white;
	}

	.title p {
		font-size: 14px;
		color: white;
		width: 160px;
		font-family: 'AbhayaLibre', sans-serif;
	}

	.wishes form {
		padding: 15px;
		width: 100%;
		border-bottom: solid 1px rgba(0, 0, 0, 0.119);
		padding-bottom: 50px;
	}

	.wishes form input {
		background-color: white;
		border: solid 1px rgba(0, 0, 0, 0.115);
		width: 100%;
		padding: 7px 10px;
		border-radius: 10px;
		margin-bottom: 15px;
	}

	.wishes form textarea {
		background-color: white;
		border: solid 1px rgba(0, 0, 0, 0.115);
		width: 100%;
		padding: 7px 10px;
		margin-bottom: 15px;
	}

	.wishes button {
		font-size: 14px;
		font-weight: bold;
		padding: 5px 20px;
		background-color: #3b5998;
		cursor: pointer;
		border-radius: 2px;
		color: white;
	}
</style>