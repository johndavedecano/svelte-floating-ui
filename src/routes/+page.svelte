<script>
	// @ts-nocheck
	import FloatingUi from '$lib/floating-ui.svelte';

	let reference;

	let floating;

	let options = { placement: 'right' };
</script>

<FloatingUi {reference} {floating} {options}>
	<button id="button" aria-describedby="tooltip" bind:this={reference}>My Button</button>
	<div id="tooltip" role="tooltip" bind:this={floating}>My Tooltip</div>
</FloatingUi>

<style>
	#tooltip {
		width: max-content;
		position: absolute;
		top: 0;
		left: 0;
		background: #222;
		color: white;
		font-weight: bold;
		padding: 5px;
		border-radius: 4px;
		font-size: 90%;
	}
</style>
