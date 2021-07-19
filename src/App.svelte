<script>
	import { onMount } from "svelte";
  import CountryData from "./CountryData.svelte";

</script>

<style lang="scss" global>
   @import "bulma/bulma";
</style>

<body class="has-background-link-light">
  <section class="hero is-small is-primary">
    <div class="hero-body">
      <p class="title">
        Domin Country Data
      </p>
      <p class="subtitle">
        Jordan Aspinall
      </p>
    </div>
  </section>
  
  <main>
    <CountryData/>
  </main>
</body>

