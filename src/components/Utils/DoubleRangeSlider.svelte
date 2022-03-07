<script>
	import { clamp } from 'yootils';
	import { createEventDispatcher } from 'svelte';

	export let start = 0;
	export let end = 1;
	let leftHandle;
	let slider;
	const dispatch = createEventDispatcher();

    const nice = d => {
        if (!d && d !== 0) return '';
        return d.toFixed(0);
    }
	function draggable(node) {
		let x;
		let y;
		function handleMousedown(event) {
			if (event.type === 'touchstart') {
				event = event.touches[0];
			}
			x = event.clientX;
			y = event.clientY;
			node.dispatchEvent(new CustomEvent('dragstart', {
				detail: { x, y }
			}));
			window.addEventListener('mousemove', handleMousemove);
			window.addEventListener('mouseup', handleMouseup);
			window.addEventListener('touchmove', handleMousemove);
			window.addEventListener('touchend', handleMouseup);
		}
		function handleMousemove(event) {
			if (event.type === 'touchmove') {
				event = event.changedTouches[0];
			}
			const dx = event.clientX - x;
			const dy = event.clientY - y;
			x = event.clientX;
			y = event.clientY;
			node.dispatchEvent(new CustomEvent('dragmove', {
				detail: { x, y, dx, dy }
			}));
		}
		function handleMouseup(event) {
			x = event.clientX;
			y = event.clientY;
			node.dispatchEvent(new CustomEvent('dragend', {
				detail: { x, y }
			}));
			window.removeEventListener('mousemove', handleMousemove);
			window.removeEventListener('mouseup', handleMouseup);
			window.removeEventListener('touchmove', handleMousemove);
			window.removeEventListener('touchend', handleMouseup);
		}
		node.addEventListener('mousedown', handleMousedown);
		node.addEventListener('touchstart', handleMousedown);
		return {
			destroy() {
				node.removeEventListener('mousedown', handleMousedown);
				node.removeEventListener('touchstart', handleMousedown);
			}
		};
	}
	function setHandlePosition (which) {
		return function (evt) {
			const { left, right } = slider.getBoundingClientRect();
			const parentWidth = right - left;
			const p = Math.min(Math.max((evt.detail.x - left) / parentWidth, 0), 1);
			if (which === 'start') {
				start = p;
				end = Math.max(end, p);
			} else {
				start = Math.min(p, start);
				end = p;
			}
			dispatch('message', {
				'start': start * 5000,
				'end': end * 5000
			});
		}
	}
</script>
<div class="header-container">
    <span class="label left">${nice(start * 5000)}</span>
    <div class="double-range-container">
        <div class="slider" bind:this={slider}>
            <div
                class="body"

                style="
                    left: {100 * start}%;
                    right: {100 * (1 - end)}%;
                "
                ></div>
            <div
                class="handle"
                bind:this={leftHandle}
                data-which="start"
                use:draggable
                on:dragmove|preventDefault|stopPropagation="{setHandlePosition('start')}"
                style="
                    left: {100 * start}%
                "
            ></div>
            <div
                class="handle"
                data-which="end"
                use:draggable
                on:dragmove|preventDefault|stopPropagation="{setHandlePosition('end')}"
                style="
                    left: {100 * end}%
                "
            ></div>
        </div>
    </div>
    <span class="label right">${nice(end * 5000)}</span>
</div>
<style type="text/scss">

    .header-container{
        width: 50%;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center; 
        margin-right: 2%;
        .label{
			color: white;
            //width: 5%;
			font-size: 30px;
			&.left{
				text-align: left;
				margin-right: 5%;
			}
			&.right{
				text-align: right;
				margin-left: 5%;
			}
        }
    }
	.double-range-container {
		width: 50%;
		height: 20px;
		user-select: none;
		box-sizing: border-box;
		white-space: nowrap
	}
	.slider {
		position: relative;
		width: 100%;
		height: 6px;
		top: 50%;
		transform: translate(0, -50%);
		background-color: #e2e2e2;
		box-shadow: inset 0 7px 10px -5px #4a4a4a, inset 0 -1px 0px 0px #9c9c9c;
		border-radius: 1px;
	}
	.handle {
		position: absolute;
		top: 50%;
		width: 0;
		height: 0;
	}
	.handle:after {
		content: ' ';
		box-sizing: border-box;
		position: absolute;
		border-radius: 50%;
		width: 16px;
		height: 16px;
		background-color: #fdfdfd;
		border: 1px solid #7b7b7b;
		transform: translate(-50%, -50%)
	}
	/* .handle[data-which="end"]:after{
		transform: translate(-100%, -50%);
	} */
	.handle:active:after {
		background-color: #ddd;
		z-index: 9;
	}
	.body {
		top: 0;
		position: absolute;
		background-color: #34a1ff;
		bottom: 0;
	}
	@media only screen and (max-width: 600px) {
	    .header-container{
		width: 100%;
		justify-content: space-around;
		.label{
			&.left{
				margin: 0;
			}
			&.right{
				margin: 0;

			}
		}
		}
	}
</style>