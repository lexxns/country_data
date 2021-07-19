<script>
    import Image from './Image.svelte'

	export let name;
	export let region;
	export let area;
	export let population;
    export let borders;
    export let flag;
</script>

<tr>
	<td>{name}</td>
    <td>{region}</td>
    <td>{area}</td>
    <td>{population}</td>
    <td>{borders.length}</td>
    <td><Image alt={name} src={flag}/></td>
</tr>