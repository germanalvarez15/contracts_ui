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
        class:movistar={card.company == 'Movistar'}
    >
        <h2>{card.title}</h2>
        <span class="company">{card.company}</span>
        <h1>${card.price}</h1>
        <ul>
            {#each card.details as detail}
                <li>{detail}</li>
            {/each}
        </ul>
        <p>{card.period}</p>
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
    .movistar{
        background-color: #99f9a5;
        border: solid 3px #8585fd;
    }
    .antel{
        background-color: #c9c9f7;
        border: solid 3px #ffff00;
    }
    .claro{
        background-color: #ffbbbb;
        border: solid 3px white;
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