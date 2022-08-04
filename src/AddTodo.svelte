<script>
    export let defaultInputValue = "";

    export let handleAddTodo = () => {};
</script>

<div>
    <div>
        <input type="text" bind:value={defaultInputValue} />
        <button
            type="button"
            on:click={handleAddTodo.bind({}, { value: defaultInputValue })}>Add Todo</button
        >
    </div>
</div>
