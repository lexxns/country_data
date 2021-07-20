<script>
  import { onMount } from "svelte";
  import { derived, writable } from "svelte/store";
  import Country from "./Country.svelte";
  import CountryBarChart from "./CountryBarChart.svelte";

  export const countryData = writable([]);

  let topBorderLength = writable([10]);
  let selectedCountryName = writable("China");

  export const topByBorders = derived(
    [countryData, topBorderLength],
    ([$countryData, $topBorderLength]) =>
      [...$countryData]
        .sort((a, b) => b.borders.length - a.borders.length)
        .slice(0, $topBorderLength)
  );

  export const selectedCountry = derived(
    [countryData, selectedCountryName],
    ([$countryData, $selectedCountryName]) =>
      $countryData
        .filter((country) => country.name === $selectedCountryName)
        .pop()
  );

  export const bordersChina = derived(selectedCountry, ($selectedCountry) =>
    $countryData
      .filter((c) => $selectedCountry.borders.includes(c.alpha3Code))
      .sort((a, b) => a.name.localeCompare(b.name))
  );

  onMount(async () => {
    const res = await fetch("https://restcountries.eu/rest/v2/all");
    const data = await res.json();

    countryData.update(() =>
      data.map((c) => ({
        name: c.name,
        region: c.region,
        area: c.area,
        population: c.population,
        flag: c.flag,
        borders: c.borders,
        alpha3Code: c.alpha3Code,
      }))
    );
  });

  function updateTopBorderLength(event) {
    topBorderLength.set(event.target.value);
  }
</script>

<div class="field">
  <label class="label">Selected Country</label>
  <div class="control">
    <div class="select">
      <select bind:value={$selectedCountryName}>
        {#each $countryData.filter((c) => c.borders.length > 0) as country}
          <option value={country.name}>
            {country.name}
          </option>
        {/each}
      </select>
    </div>
  </div>
</div>

<div class="field">
  <label class="label">Top {$topBorderLength} countries by borders</label>
  <div class="control">
    <input
      type="range"
      on:change={updateTopBorderLength}
      min="1"
      max="20"
      value="10"
    />
  </div>
</div>

<div class="tile is-ancestor is-vertical">
  <div id="borderingCountries" class="tile is-parent">
    <article class="tile is-child notification is-info">
      <p class="title">Countries that Border {$selectedCountryName}</p>
      <table class="table is-bordered is-centered is-fullwidth">
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
          {#each $bordersChina as c}
            <Country
              name={c.name}
              region={c.region}
              area={c.area}
              population={c.population}
              flag={c.flag}
              borders={c.borders}
            />
          {/each}
        </tbody>
      </table>
    </article>
  </div>

  <div id="topCountries" class="tile is-parent">
    <article class="tile is-child notification is-info">
      <p class="title">Top {$topBorderLength} countries by number of borders</p>
      <CountryBarChart
        labels={$topByBorders.map((c) => c.name)}
        borderData={$topByBorders.map((c) => c.borders.length)}
      />
    </article>
  </div>
</div>

<style lang="scss">
  .is-centered {
    margin-left: auto;
    margin-right: auto;
  }
</style>
