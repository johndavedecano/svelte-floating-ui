<script lang="ts">
	// @ts-nocheck
	import { onMount, tick, createEventDispatcher, onDestroy } from 'svelte';

	import {
		computePosition,
		autoUpdate,
		type ComputePositionConfig,
		type FloatingElement,
		type ReferenceElement
	} from '@floating-ui/dom';

	export let options: Partial<ComputePositionConfig> = {};

	export let reference: ReferenceElement;

	export let floating: FloatingElement;

	const dispatch = createEventDispatcher();

	const startComputation = () => {
		dispatch('before', { reference, floating, options });

		computePosition(reference, floating, options)
			.then(({ x, y }) => {
				Object.assign(floating.style, {
					top: `${y}px`,
					left: `${x}px`
				});
				dispatch('then', { reference, floating, options });
			})
			.catch((error) => {
				console.log(error);
				dispatch('catch', { error, reference, floating, options });
			})
			.finally(() => {
				dispatch('finally', { reference, floating, options });
			});
	};

	const setImportantElements = async () => {
		await tick();

		if (!reference) throw new Error('reference element must be a valid DOM element');

		if (!floating) throw new Error('floating element must be a valid DOM element');

		startComputation();
	};

	// const cleanup = autoUpdate(reference, floating, startComputation, options);

	// onMount(() => {
	// 	setImportantElements();
	// });

	// onDestroy(() => cleanup());

	// export const reload = () => startComputation();
</script>

<slot />
