<script>
    export let card;
</script>
    <div class="card">
        <h2>{card.nombre}</h2>
        <span class="company">{card.company}</span>
        <h1>${card.precio}</h1>
        <ul>
            {#each card.otros as detalle}
                <li>{detalle}</li>
            {/each}
        </ul>
        <p>{card.tiempo}</p>
        <button>Mas info</button>
    </div>
<style>
    .card{
        background-color: white;
        width: 30vw;
        height: auto;
        margin: 5px;
        padding: 6px;
        min-height: 500px;
    }
    h2{
        margin-bottom: 0;
    }
    .company{
        color: gray;
    }
</style>