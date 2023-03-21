<script>
  import AddTodo from "./AddTodo.svelte";
  import Todo from "./Todo.svelte";

  let todoItems = [];

  function addTodo({ detail: todo }) {
    todoItems = [...todoItems, todo];
  }

  function removeTodo({ detail: index }) {
    todoItems.splice(index, 1);
    todoItems = todoItems;
  }
</script>

<div class="container mt-5 todoBox py-4">
  <AddTodo on:addTodo={addTodo} />

  {#if todoItems.length != 0}
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
            {#each todoItems as item, index}
              <Todo {index} todo={item} on:removeTodo={removeTodo} />
            {/each}
          </tbody>
        </table>
      </div>
    </div>
    {#if todoItems.length == 1}
      <p class="text-muted mb-0 mt-2">Zbývá ti {todoItems.length} úkol</p>
    {:else if todoItems.length > 1 && todoItems.length < 10}
      <p class="text-muted mb-0 mt-2">Zbývájí ti {todoItems.length} úkoly</p>
    {:else}
      <p class="text-muted mb-0 mt-2">Zbývá ti {todoItems.length} úkolů</p>
    {/if}
  {/if}
</div>

<style>
  @media (max-width: 767px) {
    :global(.phone) {
      display: none;
    }
  }
</style>
