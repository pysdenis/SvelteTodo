<script>
  import AddTodo from "./AddTodo.svelte";
  import Todo from "./Todo.svelte";
  import Filters from "./Filters.svelte";

  let todoItems = [];
  let filteredItems = [];
  let filterStatus = "all"; 

  function addTodo({ detail: todo }) {
    todoItems = [...todoItems, todo];
    filterTodos(filterStatus);
  }

  function removeTodo({ detail: index }) {
    todoItems.splice(index, 1);
    todoItems = todoItems;
    filterTodos(filterStatus);
  }

  function filterTodos(status) {
    if (status === "all") {
      filteredItems = [...todoItems];
    } else if (status === "active") {
      filteredItems = todoItems.filter((item) => !item.status);
    } else if (status === "completed") {
      filteredItems = todoItems.filter((item) => item.status);
    }
  }

  function handleFilterChange(event) {
    filterTodos(event.detail);
  }
</script>

<div class="container mt-5 todoBox py-4">
  <AddTodo on:addTodo={addTodo} />

  

  {#if filteredItems.length != 0}
    <div class="row justify-content-center mt-4">
      <div class="col-10">
        <table class="table">
          <thead>
            <tr>
              <th scope="col">Číslo</th>
              <th scope="col">Úkol</th>
              <th class="phone" scope="col">Datum splnění</th>
              <th class="phone" scope="col">Zbývající dny</th>
              <th scope="col">Status</th>
              <th scope="col">Akce</th>
            </tr>
          </thead>
          <tbody>
            {#each filteredItems as item, index}
              <Todo {index} todo={item} on:removeTodo={removeTodo} />
            {/each}
          </tbody>
        </table>
      </div>
    </div>
    <div class="d-flex justify-content-between">
      {#if filteredItems.length == 1}
          <p class="text-muted mb-0 mt-3">Zbývá ti {filteredItems.length} úkol</p>
        {:else if filteredItems.length > 1 && filteredItems.length < 10}
          <p class="text-muted mb-0 mt-3">Zbývájí ti {filteredItems.length} úkoly</p>
        {:else}
          <p class="text-muted mb-0 mt-3">Zbývá ti {filteredItems.length} úkolů</p>
      {/if}
      <Filters on:changeFilter={handleFilterChange} />
    </div>
    {/if}
</div>

<style>
  @media (max-width: 767px) {
    :global(.phone) {
      display: none;
    }
  }
</style>