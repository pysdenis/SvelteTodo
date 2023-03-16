<script>
  import AddTodo from "./AddTodo.svelte";
  import Todo from "./Todo.svelte";

  let todoItems = [];
  //let doneTasks = 0;

  // { detail: todo }
  // Do add todo totiž vleze objekt, ve kterém nás zajímá objekt "detail" (to co jsme poslali přes dispatch)
  // : todo -> přejmenovává detail na "todo"
  function addTodo({ detail: todo }) {
    todoItems = [...todoItems, todo];
  }

  // Jak by se to zapsalo bez těch chytrostí nad
  // function addTodo(event) {
  //   todoItems = [...todoItems, event.detail];
  // }

  function removeTodo({ detail: index }) {
    todoItems.splice(index, 1);
    todoItems = todoItems;
  }
</script>

<div class="container mt-5 todoBox py-4">
  <!-- Při eventu addTodo z komponenty AddTodo se zavolá funkce addTodo (z této komponenty) -->
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
              <!--
                Rozdělil jsem AddTodo a Todo do zvlášť komponent, ať se starají o svoje věci a zmenší tuto komponentu
                Napodobně fungující event "removeTodo" zde

                Všimni si využití "atributů" u komponenty (index a todo)

                Pro atribut index je využitý "shorthand"
                (jelikož proměnná a atribut se nazývají stejně, tak to lze napsat takto rovnou celé uvnitř {})
                (je to to stejné jako index={index})
              -->
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
