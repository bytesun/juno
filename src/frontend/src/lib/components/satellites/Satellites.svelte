<script lang="ts">
	import SatelliteArticle from '$lib/components/satellites/SatelliteArticle.svelte';
	import { satellitesStore } from '$lib/stores/satellite.store';
	import SatelliteNew from '$lib/components/satellites/SatelliteNew.svelte';
	import type { Satellite } from '$declarations/mission_control/mission_control.did';
	import SatellitesToolbar from '$lib/components/satellites/SatellitesToolbar.svelte';
	import { satelliteName } from '$lib/utils/satellite.utils';
	import { layoutSatellites } from '$lib/stores/layout.store';
	import { SatellitesLayout } from '$lib/types/layout';

	let filter = '';

	let satellites: Satellite[];
	$: satellites = ($satellitesStore ?? []).filter(
		(satellite) =>
			satelliteName(satellite).toLowerCase().includes(filter.toLowerCase()) ||
			satellite.satellite_id.toText().includes(filter.toLowerCase())
	);
</script>

<SatellitesToolbar bind:filter />

<SatelliteNew row={$layoutSatellites === SatellitesLayout.LIST} />

{#each satellites as satellite}
	<SatelliteArticle {satellite} />
{/each}
