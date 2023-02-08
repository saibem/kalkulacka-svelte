<script>
    export let options;
    export let onChange;

    function handleChange(event) {
        console.log('here')
        if (event === null) return;
        onChange(event.currentTarget.value);
    }
</script>

<select on:change={handleChange}>
    {#each options as option}
        <option value={option.value}>{option.label}</option>
    {/each}
</select>