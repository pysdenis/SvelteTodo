<script>
    let todoItems = [];
    let newTodo = '';
    let newDeadline = '';
    //let doneTasks = 0;
    
    function addTodo() {
      if (newTodo === '') {
        alert("Vyplň prosím název úkolu.")
        return;
      }
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

<div class="container mt-5 todoBox py-4">
  <div class="row justify-content-center">
    <div class="col-10 col-md-8 col-lg-6">
      <form>
        <h3 class="text-center mb-4">Co je potřeba udělat?</h3>
        <div class="form-floating mb-3">
          <input type="text" bind:value={newTodo} class="form-control" id="nameTodo" placeholder="Jméno úkolu.."/>
          <label for="nameTodo">Jméno úkolu..</label>
        </div>
        <div class="form-floating mb-3">
          <input name="date" type="date" bind:value={newDeadline} class="form-control" id="floatingDate"/>
          <label for="floatingDate">Datum splnění:</label>
        </div>
        <button on:click={addTodo} class="btn btn buttonSubmit w-100">Přidat</button>
      </form>
    </div>
  </div>
  
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
              <tr>
                <td class="text-muted">{index + 1}</td>
                <td class:checked={item.status}><strong>{item.text}</strong></td>
                <td class="phone">{item.deadline}</td>
                <td class="phone">{getDaysLeft(item.deadline)} dnů</td>
                {#if item.status == false}
                  <td>nedokončeno</td>
                {:else if item.status == true}
                  <td>dokončeno</td>
                {/if}
                <td>
                  <button class="btn btn-danger me-2" on:click={() => removeFromList(index)}>Smazat</button>
                  <div class="form-check form-switch d-inline-block">
                    <input type="checkbox" class="form-check-input" id={"checkbox" + index} bind:checked={item.status}/>
                    <label class="form-check-label" for={"checkbox" + index}>Dokončit</label>
                  </div>
                </td>
              </tr>
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
  .checked {
        text-decoration: line-through;
  }

  @media (max-width: 767px) {
    .phone{
      display: none;
    } 
  }


</style>