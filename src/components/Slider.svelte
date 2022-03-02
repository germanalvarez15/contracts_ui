<script>
    const onSliderChange = (event) => {
        console.log(event.target.value);
    }
</script>
<div class="slider-container">
    <span>0</span>
    <input type="range" min="0" max="5000" step="100" on:change={onSliderChange}>
    <span>5000</span>
</div>
<style>
    input{
        margin: 0;
        padding: 0;
    }
</style>