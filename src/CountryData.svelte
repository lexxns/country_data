<script>
	import { onMount } from 'svelte';
	import { derived, writable } from 'svelte/store';
    import Country from './Country.svelte';


	export const countryData = writable([]);
	
	export const topTenByBorders = derived(
		countryData,
		$countryData => $countryData.sort((a, b) => b.borders.length - a.borders.length).slice(0, 10)
	);
	
	export const china = derived(
		topTenByBorders,
		$topTenByBorders => $topTenByBorders.filter(country => country.name === 'China').pop()
	);
	
	export const bordersChina = derived(
		china,
		$china => $countryData.filter(c => $china.borders.includes(c.alpha3Code))
							  .sort((a, b) => a.name.localeCompare(b.name))
	);
	

	onMount(async () => {
		const res = await fetch('https://restcountries.eu/rest/v2/all');
		const data = await res.json();

		countryData.update(() => data.map(c => ({
			name: c.name,
			region: c.region,
			area: c.area,
			population: c.population,
			flag: c.flag,
			borders: c.borders,
			alpha3Code: c.alpha3Code,
		})));
	});

</script>
<h2>Countries that Border China</h2>
<div class="container">
	
	<table class="table is-bordered">
		<thead>
			<tr>
				<th>Name</th>
				<th>Region</th>
				<th>Area</th>
				<th>Population</th>
				<th>Borders</th>
				<th>Flag</th>
			</tr>
		</thead>
		<tbody>
			{#each $bordersChina as c }
			<Country name={c.name}
				region={c.region}
				area={c.area}
				population={c.population}
				flag={c.flag}
				borders={c.borders}
			/>
		{/each}
		</tbody>

	</table>
</div>
