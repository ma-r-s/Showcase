<script>
	import { Canvas } from '@threlte/core';
	import Scene from './Scene.svelte';
</script>

<h1 class="text-3xl text-white font-bold m-5">FFT MX</h1>

<div class="w-full grow">
	<Canvas frameloop={'always'}>
		<Scene />
	</Canvas>
</div>
