<script lang="ts">
	import { SelectedCandidateStore } from '$lib/stores/Candidates';
	import { CandidateCounts } from '$lib/stores/regions/Regions';

	import { calculateLumaHEX } from '$lib/utils/luma';
	import type Candidate from '$lib/types/Candidate';

	export let candidate: Candidate;
	export let selectable: boolean;
	export let transitions = true;

	$: selected = selectable && $SelectedCandidateStore.id === candidate.id;
	$: textColor = calculateLumaHEX(candidate.margins[0].color) > 0.5 ? 'black' : 'white';
	$: backgroundColor = candidate.margins[0].color;
	$: transitionSpeed = transitions ? '150ms' : '0ms';

	function updateSelectedCandidate() {
		SelectedCandidateStore.set(candidate);
	}
</script>

<div class="btn-group p-0.5 pointer-events-auto">
	<button
		class="btn no-animation overflow-hidden p-0 transition-all"
		class:btn-sm={selected}
		class:btn-xs={!selected}
		style:transition-duration={transitionSpeed}
		style:background-color={backgroundColor}
		style:color={textColor}
		on:click={updateSelectedCandidate}
	>
		<div class="flex flex-col gap-0">
			<div class="pt-1 pb-1 pl-2 pr-2">
				{candidate.name}
				{$CandidateCounts.get(candidate.id) ?? 0}
			</div>
			{#if candidate.margins.length > 1}
				<div class="flex flex-row w-full">
					{#each candidate.margins as margin}
						<div class="w-full min-h-6" style:background-color={margin.color} />
					{/each}
				</div>
			{/if}
		</div>
	</button>
</div>
