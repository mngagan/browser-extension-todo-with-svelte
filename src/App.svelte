<script>
  let UNIQUE_IDENTIFIER = "f65caf9d-3c89-431e-9771-8bcfaded4ed4";
  import AddToDo from "./AddTodo.svelte";
  import { onMount } from "svelte";

  let uuid = () => Math.random().toString(36).substr(2, 9);

  let todoData = [];
  let isDataFetched = false;
  let defaultInputValue = "";
  onMount(() => {
    let storedData = window.localStorage.getItem(UNIQUE_IDENTIFIER);
    isDataFetched = true;
    if (storedData) {
      todoData = JSON.parse(storedData);
    }
  });

  const handleAddTodo = ({ value }) => {
    todoData = [...todoData, { task: value, status: false, id: uuid() }];
    window.localStorage.setItem(UNIQUE_IDENTIFIER, JSON.stringify(todoData));
    defaultInputValue = "";
  };

  const updateDataToLocalStorage = () =>
    window.localStorage.setItem(UNIQUE_IDENTIFIER, JSON.stringify(todoData));

  const handleDeleteTask = ({ task }) => {
    todoData = todoData.filter((eachTodo) => eachTodo.id !== task.id);
    // updateDataToLocalStorage()
  };

  $: {
    isDataFetched && todoData && updateDataToLocalStorage();
  }
</script>

<div class="container border border-red-300 p-2">
  <div class="grid grid-cols-1 gap-4">
    <div class="text-center text-lg text-center">Todo List</div>
    <div>
      <AddToDo {handleAddTodo} bind:defaultInputValue />
    </div>
    <div>to display list of todos</div>
    <div class="max-height-350 grid grid-cols-1 gap-4">
      {#each todoData as task}
        <div
          class={`hover:bg-yellow-300 transition-colors duration-300 ${
            task.status ? "bg-green-300" : ""
          }`}
        >
          <span class="width80pt border border-green-300">
            <label
              ><input type="checkbox" bind:checked={task.status} />
              {task.task}
            </label>
          </span>
          <span class="width20pt" on:click={handleDeleteTask.bind({}, { task })}
            >Delete</span
          >
        </div>
      {/each}
    </div>
  </div>
</div>

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  /* purgecss end ignore */
  @tailwind utilities;

  @layer utilities {
    .ml-55 {
      margin-left: 55%;
    }
  }
  .container {
    width: 500px;
    height: 500px;
  }
  .max-height-350 {
    max-height: 350px;
    overflow: auto;
  }
  .width80pt {
    width: 80%;
  }
  .width20pt {
    width: 20%;
  }
</style>
