<script>
    export let card;
    const showInfo = () => {
        window.open(card.link, '_blank');
    }
</script>
    <div 
        class="card"
        class:claro={card.company === 'Claro'}
        class:antel={card.company === 'Antel'}
    >
        <h2>{card.nombre}</h2>
        <span class="company">{card.company}</span>
        <h1>${card.precio}</h1>
        <ul>
            {#each card.otros as detalle}
                <li>{detalle}</li>
            {/each}
        </ul>
        <p>{card.tiempo}</p>
        <button on:click={showInfo}>Mas info</button>
    </div>
<style>
    .card{
        background-color: white;
        border-radius: 15px;
        width: 30vw;
        height: auto;
        margin: 5px;
        padding: 6px;
        min-height: 500px;
    }
    .antel{
        background-color: #ffffbb;
        border: solid 2px #8585fd;
    }
    .claro{
        background-color: #ffbbbb;
        border: solid 2px white;
    }
    h2{
        margin-bottom: 0;
    }
    .company{
        color: gray;
    }
    @media only screen and (max-width: 600px) {
        .card{
            width: 80vw;
        }
    }
</style>