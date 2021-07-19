<script>
    export let src;
    export let alt;
</script>

<style >
    img {
        max-height: 200px;
        max-width: 200px;
    }
</style>

<figure class="image is-4by3">
    <img class="is-rounded" src={src} {alt}>
</figure>