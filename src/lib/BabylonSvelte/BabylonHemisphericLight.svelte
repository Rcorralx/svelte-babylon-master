<script lang="ts">
	import type { HemisphericLight, Vector3 } from 'babylonjs';

	import { getContext, onMount, onDestroy } from 'svelte';

	const { getScene } = getContext('BabylonScene');
	const scene = getScene();

	let light: HemisphericLight;

	export let name = '';
	$: if (name && light) light.name = name;

	export let direction: Vector3;
	$: if (direction && light) light.direction = direction;

	export let intensity: number;
	$: if (intensity && light) light.intensity = intensity;

	let BABYLON: typeof import('babylonjs');

	$: if (BABYLON && $scene && !light) {
		if (!direction) direction = new BABYLON.Vector3(0, 0, 0);
		if (!intensity) intensity = 1.0;

		light = new BABYLON.HemisphericLight(name, direction, $scene);
	}

	onMount(async () => {
		const babylonjs = await import('babylonjs');
		BABYLON = babylonjs.default;
	});

	onDestroy(() => {
		if ($scene && light) $scene.removeLight(light);
	});
</script>

<slot />
