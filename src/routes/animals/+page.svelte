<script>
	import { cat, cow } from './animals.js';
	import { interpolate } from 'polymorph-js';
	import { draw } from 'svelte/transition';
	import { sineInOut } from 'svelte/easing';
	import { tweened } from 'svelte/motion';
	let drawn = false;
	let look = true;

	const inter = (a, b) => (t) => interpolate([a, b])(t);
	const path = tweened(cow[0], {
		duration: 800,
		interpolate: inter,
		easing: sineInOut
	});
</script>

<div class="flex gap-5">
	<button
		on:click={() => {
			drawn = !drawn;
		}}
		class="rounded full p-2 bg-green-500 text-white"
	>
		Draw
	</button>
	<button
		on:click={() => {
			if (look) path.set(cow[1]);
			else path.set(cat[0]);
			look = !look;
		}}
		class="rounded full p-2 bg-red-500 text-white"
	>
		Morph
	</button>
</div>
<svg class="w-1/3" viewBox="0 0 800 600">
	{#if drawn}
		<path
			transition:draw={{ duration: 3000, easing: sineInOut }}
			d={$path}
			fill="none"
			opacity="1"
			stroke="#ffffff"
			stroke-linecap="round"
			stroke-linejoin="round"
			stroke-width="12"
		/>
	{/if}
</svg>
