<script>
import Card from "./components/card.svelte";
import Slider from "./components/Slider.svelte";
import { onMount } from 'svelte';

	const contratos = (async () => {
		const response = await fetch('http://localhost:5000/contratos');
		const responseFiltered = await response.json();
		responseFiltered.sort(function (a, b) {
			if (a.precio > b.precio) {
				return 1;
			}
			if (a.precio < b.precio) {
				return -1;
			}
			// a must be equal to b
			return 0;
		});
		console.log(responseFiltered);
		return responseFiltered
	})()
</script>

<main>
	<Slider></Slider>
	{#await contratos}
		<p>...waiting</p>
	{:then data}
		<div class="cards-container">
			{#each data as contr}
				<Card card={contr}/>
			{/each}
			
		</div>
	{:catch error}
		<p>An error occurred! {error}</p>
	{/await}

</main>

<style>
	:global(body){
		background-color: lightgray;
	}
	.cards-container{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: space-around;
		align-content: space-around;
		align-items: center;
	}
</style>