<script>
    let todoItems = [];
    let newTodo = '';
    let newDeadline = '';
    
    function addTodo() {
      todoItems = [...todoItems, {text: newTodo, status: false, deadline: newDeadline}];
      newTodo = '';
      newDeadline = '';
    }

    function removeFromList(index) {
      todoItems.splice(index, 1)
      todoItems = todoItems;
    }

    function getDaysLeft(deadline) {
      const now = new Date();
      const deadlineDate = new Date(deadline);
      const timeDiff = deadlineDate.getTime() - now.getTime();
      return Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
    }


</script>

<div class="row mt-5 todo">
  <div class="w-50 mx-auto">
    <div>
      <form>
        <h3 class="text-center">Co je třeba udělat?</h3>
        <div class="d-flex align-items-center">
          <div class="form-floating me-2 w-50">
            <input type="text" bind:value={newTodo} class="form-control" id="floatingTodo" placeholder="Jméno úkolu.."/>
            <label for="floatingTodo">Jméno úkolu..</label>
          </div>
          <div class="form-floating me-2 w-50">
            <input name="date" type="date" bind:value={newDeadline} class="form-control" id="floatingDate"/>
            <label for="floatingDate">Datum splnění:</label>
          </div>
          <button on:click={addTodo} class="btn buttonSubmit">Přidat</button>
        </div>
      </form>
    </div>
  </div>
</div>


{#if todoItems.length != 0}
  <table class="table mt-5 w-75 mx-auto">
    <thead>
      <tr>
        <th scope="col">Číslo</th>
        <th scope="col">Úkol</th>
        <th scope="col">Termín splnění</th>
        <th scope="col">Počet dnů do termínu splnění</th>
        <th scope="col">Status</th>
        <th scope="col">Akce</th>
      </tr>
    </thead>
    <tbody>
      {#each todoItems as item, index}
        <tr>
          <th scope="row">{index + 1}</th>
          <td class:checked={item.status}>{item.text}</td>
          <td>{item.deadline}</td>
          <td>{getDaysLeft(item.deadline)} dnů</td>
          {#if item.status == false}
            <td>nedokončeno</td>
          {:else if item.status == true}
            <td>dokončeno</td>
          {/if}
          <td>
            <button class="btn btn-danger" on:click={() => removeFromList(index)}>Smazat</button>
            <div>
              <input type="checkbox" class="form-check-input" id="checkbox" bind:checked={item.status}/>
              <label class="form-check-label" for="checkbox">Dokončit</label>
            </div>
          </td>
        </tr>
      {/each} 
    </tbody>
</table>
{/if}


<style>
  .checked {
        text-decoration: line-through;
    }

</style>