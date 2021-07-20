<script>
  import { onMount } from "svelte";
  import { derived, writable } from "svelte/store";
  import Country from "./Country.svelte";
  import CountryBarChart from "./CountryBarChart.svelte";

  export const countryData = writable([]);

  let topBorderLength = writable([10]);

  export const topByBorders = derived(
    [countryData, topBorderLength],
    ([$countryData, $topBorderLength]) =>
      $countryData
        .sort((a, b) => b.borders.length - a.borders.length)
        .slice(0, $topBorderLength)
  );

  export const china = derived(topByBorders, ($topByBorders) =>
    $topByBorders.filter((country) => country.name === "China").pop()
  );

  export const bordersChina = derived(china, ($china) =>
    $countryData
      .filter((c) => $china.borders.includes(c.alpha3Code))
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

<div class="tile is-ancestor is-vertical">
  <div class="tile is-parent">
    <article class="tile is-child notification is-info">
      <p class="title">Countries that Border China</p>
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
  <div class="tile is-parent">
    <article class="tile is-child notification is-info">
      <p class="title">Top {$topBorderLength} countries by number of borders</p>
      <input
        type="range"
        on:change={updateTopBorderLength}
        min="1"
        max="20"
        value="10"
      />
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
