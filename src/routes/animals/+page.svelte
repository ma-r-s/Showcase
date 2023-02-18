<script>
	import { cat, cow } from './animals.js';
	import { interpolate } from 'polymorph-js';
	import { draw } from 'svelte/transition';
	import { sineInOut } from 'svelte/easing';
	import { tweened } from 'svelte/motion';
	let drawn = false;
	let look = true;

	const inter = (a, b) => (t) => interpolate([a, b], { optimize: 'none' })(t);
	const catPath = tweened(cat[0], {
		duration: 800,
		interpolate: inter,
		easing: sineInOut
	});
	const cowPath = tweened(cow[0], {
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
			if (look) catPath.set(cat[1]) && cowPath.set(cow[1]);
			else catPath.set(cat[0]) && cowPath.set(cow[0]);
			look = !look;
		}}
		class="rounded full p-2 bg-red-500 text-white"
	>
		Morph
	</button>
</div>
<svg viewBox="0 0 1600 600">
	{#if drawn}
		<path
			transition:draw={{ duration: 3000, easing: sineInOut }}
			d={$catPath}
			fill="none"
			opacity="1"
			stroke="#ffffff"
			stroke-linecap="round"
			stroke-linejoin="round"
			stroke-width="12"
		/>
		<path
			transition:draw={{ duration: 3000, easing: sineInOut }}
			transform="translate(800, 0)"
			d={$cowPath}
			fill="none"
			opacity="1"
			stroke="#ffffff"
			stroke-linecap="round"
			stroke-linejoin="round"
			stroke-width="12"
		/>
	{/if}
</svg>
