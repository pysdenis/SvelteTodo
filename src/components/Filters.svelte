<script>
  import { createEventDispatcher } from "svelte"; 
  export let filterStatus = "all"; 

  const dispatch = createEventDispatcher(); 

  function filterTodos(status) {
    filterStatus = status;
    dispatch("changeFilter", filterStatus);
  }
</script>

<div class="d-flex justify-content-end mt-2">
  <div class="btn-group btn-group-sm">
    <button
      class="btn btn-outline-secondary"
      class:selected={filterStatus === "all"}
      on:click={() => filterTodos("all")}
    >
      Všechny
    </button>

    <button
      class="btn btn-outline-secondary"
      class:selected={filterStatus === "active"}
      on:click={() => filterTodos("active")}
    >
      Nedokončené
    </button>

    <button
      class="btn btn-outline-secondary"
      class:selected={filterStatus === "completed"}
      on:click={() => filterTodos("completed")}
    >
      Dokončené
    </button>

  </div>
</div>