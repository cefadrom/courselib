<script>
    export let label;
    export let value;
    export let noMB = false;

    let classes;
    $: classes = `${noMB ? '' : 'mb-4'}\
        px-4 py-2 w-full rounded border border-gray-400 focus:outline-none focus:border-blue-400 shadow-none\
        ${$$props.class || ''}`;
</script>

<label>
    {#if label}
        <span class="block mb-1">{label}</span>
    {/if}
    <input {...$$props} class={classes} bind:value/>
</label>
