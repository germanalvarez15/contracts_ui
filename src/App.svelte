<script>
import Card from "./components/CardItem.svelte";
import Header from "./components/HeaderContainer.svelte";

	let contratos;
	const handleMessage = (event) =>{
		contratosPromise.then(data =>{
			contratos = data.filter(c => c.price > event.detail.start && c.price <= event.detail.end);

		},error =>{
			console.error(error);
		})
	}

	const contratosPromise = (async () => {
		const response = await fetch(API_URL + CONTRATOS);
		const responseFiltered = await response.json();
		responseFiltered.sort(function (a, b) {
			if (a.price > b.price) {
				return 1;
			}
			if (a.price < b.price) {
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
	{#await contratosPromise}
		<div class="loader-container">
			<div class="loader"></div>
		</div>
	{:then data}
		<Header on:message={handleMessage}></Header>
		<div class="cards-container">
			{#each contratos as contr}
				<Card card={contr}/>
			{/each}
			
		</div>
	{:catch error}
		<div class="error-container">
			<img src="https://mumbrella.com.au/wp-content/uploads/2017/10/technical_difficulties_simpsons.8d0bd724041bb166adce44e95715c12e.jpg" alt="Waiting"/>
			<p>An error occurred! {error}</p>
		</div>
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
	.loader-container,
	.error-container{
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.loader {
		border: 16px solid #f3f3f3; /* Light grey */
		border-top: 16px solid #3498db; /* Blue */
		border-radius: 50%;
		width: 120px;
		height: 120px;
		animation: spin 2s linear infinite;
	}

	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}
</style>