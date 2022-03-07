<script>
import Card from "./components/card.svelte";
import Header from "./components/header.svelte";

	let contratos;
	const handleMessage = (event) =>{
		contratosPromise.then(data =>{
			contratos = data.filter(c => c.precio > event.detail.start && c.precio <= event.detail.end);

		},error =>{
			console.error(error);
		})
	}

	const contratosPromise = (async () => {
		const response = await fetch(API_URL + CONTRATOS);
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
		contratos = responseFiltered;
		return responseFiltered
	})()
</script>

<main>
	<Header on:message={handleMessage}></Header>
	{#await contratosPromise}
		<p>...waiting</p>
	{:then data}
		<div class="cards-container">
			{#each contratos as contr}
				<Card card={contr}/>
			{/each}
			
		</div>
	{:catch error}
		<p>An error occurred! {error}</p>
	{/await}

</main>

<style>
	
	:global(body){
		background-color: #424242;
	}
	p{
		color: white;
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