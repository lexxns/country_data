<script>
    import { onMount } from 'svelte';
    export let src;
    export let alt;

    let loaded = false;
    let thisImage;

    onMount(() => {
        thisImage.onload = () => {
        loaded = true
        }
    });

</script>

<style >
    img {
        max-height: 200px;
        max-width: 200px;
    }
</style>

<img {src} {alt} class:loaded bind:this={thisImage} loading="lazy" />