<script>
  import CountryData from "./CountryData.svelte";
</script>

<body class="has-background-link-light">
  <section class="hero is-small is-primary">
    <div class="hero-body">
      <p class="title">Domin Country Data</p>
      <p class="subtitle">Jordan Aspinall</p>
    </div>
  </section>

  <section class="section">
    <main>
      <CountryData />
    </main>
  </section>
</body>

<style lang="scss" global>
  @import "bulma/bulma";
</style>
