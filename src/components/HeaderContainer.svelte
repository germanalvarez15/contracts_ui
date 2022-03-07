<script>
    import DoubleRangeSlider from "./Utils/DoubleRangeSlider.svelte";
    import { createEventDispatcher } from 'svelte';

    let start;
    let end;
    const dispatch = createEventDispatcher();

    function handleMessage(event) {
        dispatch('message', {
            'start': event.detail.start,
            'end': event.detail.end
        });
	}
</script>
<div class="main">
    <img alt="logo" src="assets/logo.png"/>
    <!--<Slider></Slider>-->
	<DoubleRangeSlider bind:start bind:end on:message={handleMessage}/>

</div>
<style>
.main{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-content: center;
    align-items: center;
    justify-content: space-between;
}
img{
    width: 15%;
    height: inherit;
}
@media only screen and (max-width: 600px) {
    .main{
        flex-direction: column;
    }
    img{
        width: 35%;
    }
}

</style>