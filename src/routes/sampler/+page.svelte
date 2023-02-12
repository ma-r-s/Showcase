<script>
	import { browser } from '$app/environment';
	import { ArrowSmallUp, Pause, Play, Stop } from '@steeze-ui/heroicons';
	import { Icon } from '@steeze-ui/svelte-icon';
	import * as Tone from 'tone';
	let notes = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];
	const nestedArray = Array.from(Array(12), () => Array(16).fill(false));
	const add = (i, j) => () => {
		nestedArray[i][j] = !nestedArray[i][j];
	};
	let playing = false;
	let bpm = 120;
	let currentChord = -1;
	let synth;
	let a = 0.5;
	let d = 0.5;
	let s = 0.5;
	let r = 0.5;
	$: sum = a + d + s + r;
	$: ra = (a * 80) / sum + 10;
	$: rd = ((a + d) * 80) / sum + 10;
	$: rs = ((a + d + s) * 80) / sum + 10;

	if (browser) {
		// it uses ampEvn to control the volume
		synth = new Tone.PolySynth(Tone.Synth, {
			envelope: {
				attack: a,
				decay: d,
				sustain: s,
				release: r
			}
		}).toDestination();
	}
	let nextChord = () => {
		if (playing) {
			currentChord = (currentChord + 1) % 16;
			for (let i = 0; i < 12; i++) {
				if (nestedArray[i][currentChord]) {
					synth.triggerAttackRelease(notes[i] + '4', '8n');
				}
			}
		}
		setTimeout(() => {
			nextChord();
		}, 60000 / bpm);
	};
	nextChord();
</script>

<div class="flex w-4/5 justify-between items-center">
	<p class="text-7xl">SMPL</p>
	<div class="flex gap-2 justify-center">
		<div class="text-4xl">BPM: {bpm}</div>
		<button on:click={() => (bpm += 1)}>
			<Icon src={ArrowSmallUp} theme="outline" class="h-8" />
		</button>
		<button on:click={() => (bpm -= 1)}>
			<Icon src={ArrowSmallUp} theme="outline" class="h-8 transform rotate-180" />
		</button>
	</div>
	<div class="flex gap-5">
		<button on:click={() => (playing = true)}>
			<Icon src={Play} theme="outline" class="h-8 stroke-green-500" />
		</button>
		<button on:click={() => (playing = false)}>
			<Icon src={Pause} theme="outline" class="h-8 " />
		</button>
		<button
			on:click={() => {
				currentChord = -1;
				playing = false;
			}}
		>
			<Icon src={Stop} theme="outline" class="h-8 stroke-red-600" />
		</button>
	</div>
</div>
<div class="flex w-full">
	<div
		class=" w-2/3 grid grid-cols-[repeat(17,minmax(0,1fr))] border gap-3 p-4 justify-items-center"
	>
		<div />
		{#each Array(16) as _, n}
			<div>{n + 1}</div>
		{/each}
		{#each notes as note, i}
			<p class="text-center text-2xl font-bold">{note}</p>
			{#each Array(16) as _, j}
				<button
					class="aspect-square border text-center hover:bg-zinc-700 hover:text-white {nestedArray[
						i
					][j]
						? 'bg-white text-black'
						: ''} {j == currentChord ? 'border-red-600 text-black' : ''}"
					on:click={add(i, j)}
				/>
			{/each}
		{/each}
	</div>
	<div class="border w-1/3">
		<svg
			class="border m-5"
			viewBox="0 0 100 100"
			xmlns="http://www.w3.org/2000/svg"
			stroke-linecap="round"
		>
			<line x1={ra} y1="10" x2={ra} y2="90" class="stroke-white" stroke-dasharray="1,2" />
			<line x1={rd} y1="50" x2={rd} y2="90" class="stroke-white" stroke-dasharray="1,2" />
			<line x1={rs} y1="50" x2={rs} y2="90" class="stroke-white" stroke-dasharray="1,2" />
			<line x1="10" y1="90" x2={ra} y2="10" class="stroke-red-500 stroke-2" />
			<line x1={ra} y1="10" x2={rd} y2="50" class="stroke-orange-500 stroke-2" />
			<line x1={rd} y1="50" x2={rs} y2="50" class="stroke-green-500 stroke-2" />
			<line x1={rs} y1="50" x2="90" y2="90" class="stroke-purple-500 stroke-2" />
			<circle cx={ra} cy="10" r="2" class="stroke-white" />
			<circle cx={rd} cy="50" r="2" class="stroke-white" />
			<circle cx={rs} cy="50" r="2" class="stroke-white" />
			<line x1="5" y1="90" x2="95" y2="90" class="stroke-white stroke-2" />
		</svg>
		<!-- Four range input for ADSR -->
		<div class="flex flex-col gap-4 p-4">
			<div class="flex justify-between">
				<label for="attack">Attack</label>
				<input type="range" bind:value={a} min="0.01" max="1" step="0.01" />
			</div>
			<div class="flex justify-between">
				<label for="decay">Decay</label>
				<input type="range" bind:value={d} min="0.01" max="1" step="0.01" />
			</div>
			<div class="flex justify-between">
				<label for="sustain">Sustain</label>
				<input type="range" bind:value={s} min="0.01" max="1" step="0.01" />
			</div>
			<div class="flex justify-between">
				<label for="release">Release</label>
				<input type="range" bind:value={r} min="0.01" max="1" step="0.01" />
			</div>
		</div>
	</div>
</div>
