<script>
	import { Icon } from '@steeze-ui/svelte-icon';
	import { Play, Pause, Stop, ArrowSmallUp } from '@steeze-ui/heroicons';
	let notes = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];
	const nestedArray = Array.from(Array(12), () => Array(16).fill(false));
	const add = (i, j) => () => {
		nestedArray[i][j] = !nestedArray[i][j];
	};
	let bpm = 120;
</script>

<div class="flex w-4/5 justify-between items-center">
	<p class="text-7xl">SMPL</p>
	<div class="flex justify-end gap-20">
		<div class="flex gap-4">
			<div class="text-4xl">BPM: {bpm}</div>
			<button on:click={() => (bpm += 1)}>
				<Icon src={ArrowSmallUp} theme="outline" class="h-8" />
			</button>
			<button on:click={() => (bpm -= 1)}>
				<Icon src={ArrowSmallUp} theme="outline" class="h-8 transform rotate-180" />
			</button>
		</div>
		<div class="flex gap-5">
			<Icon src={Play} theme="outline" class="h-8 stroke-green-500" />
			<Icon src={Pause} theme="outline" class="h-8 " />
			<Icon src={Stop} theme="outline" class="h-8 stroke-red-600" />
		</div>
	</div>
</div>
<div
	class="grid grid-cols-[repeat(16,minmax(0,1fr))] grid-rows-[repeat(13,minmax(0,1fr))] border gap-4 p-4"
>
	{#each Array(16) as _, n}
		<p class="text-center text-2xl font-bold">{n + 1}</p>
	{/each}
	{#each notes as note, i}
		{#each Array(16) as _, j}
			<button
				class="py-1 border text-center w-16 hover:bg-zinc-700 hover:text-white {nestedArray[i][j]
					? 'bg-white text-black'
					: ''}"
				on:click={add(i, j)}>{note}</button
			>
		{/each}
	{/each}
</div>
