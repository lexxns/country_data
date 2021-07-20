<script>
  export let src;
  export let alt;
</script>

<figure class="image is-4by3">
  <img {src} {alt} />
</figure>

<style>
  img {
    max-height: 200px;
    max-width: 200px;
    /* min-height: 200px;
    min-width: 200px; */
  }
</style>
