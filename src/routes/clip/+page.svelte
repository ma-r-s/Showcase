<script>
	let vol = 2;
</script>

<div class=" grow flex w-full justify-center items-center">
	<div class="w-1/4 flex  flex-col-reverse gap-2 items-center ">
		{#each Array(12) as _, i}
			<button
				on:click={() => (vol = i)}
				class="rounded-full text-white  w-16 h-4 {vol < i ? 'bg-slate-100' : 'bg-red-500'}"
			/>
		{/each}
	</div>
	<div class="flex-col flex w-1/2">
		<svg class="border" viewBox="0,0,400,300">
			{#each Array(5) as _, i}
				<line y1="300" x1="100" x2="200" stroke="white" stroke-width="2" />
			{/each}
		</svg>
	</div>
	<div class="w-1/4 flex flex-col-reverse gap-2 items-center">
		{#each Array(12) as _, i}
			<button
				on:click={() => (vol = i)}
				class="rounded-full text-white w-16 h-4 {vol < i ? 'bg-slate-100' : 'bg-red-500'}"
			/>
		{/each}
	</div>
</div>
