<script lang="ts">
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';


	interface AgifyResponse {
		name: string;
		age: number;
		count?: number;
		error?: string;
	}


	let nome = '';
	let responseData: Partial<AgifyResponse> = {};

	async function handleSearch() {
		const url = `https://api.agify.io/?name=${nome}`;
		let response = await fetch(url);
		let data = await response.json();
		responseData = data;
	}

</script>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		Digite um nome e veja sua idade estimada

		<input type="text" bind:value={nome} />
		<button on:click={handleSearch}>VAI NOMe</button>

		{#if responseData.age}
			<p>Idade estimada: {responseData.age}</p>
		{:else if responseData.error || responseData.age === null}
			<p>Esse nome não é bao</p>
		{/if}
	</h1>

</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
