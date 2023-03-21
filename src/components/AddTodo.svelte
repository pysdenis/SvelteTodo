<script>
	import { createEventDispatcher } from "svelte";

	let text = "";
	let deadline = "";

	const dispatch = createEventDispatcher();

	function newTodo() {
		if (text === "") {
			alert("Vyplň prosím název úkolu.");
			return;
		}

		dispatch("addTodo", {
			text,
			deadline,
			status: false,
		});

		text = "";
		deadline = "";
	}
</script>

<div class="row justify-content-center">
	<div class="col-10 col-md-8 col-lg-6">
		<form>
			<h3 class="text-center mb-4">Co je potřeba udělat?</h3>
			<div class="form-floating mb-3">
				<input
					type="text"
					bind:value={text}
					class="form-control"
					id="nameTodo"
					placeholder="Jméno úkolu.."
				/>
				<label for="nameTodo">Jméno úkolu..</label>
			</div>
			<div class="form-floating mb-3">
				<input
					name="date"
					type="date"
					bind:value={deadline}
					class="form-control"
					id="floatingDate"
				/>
				<label for="floatingDate">Datum splnění:</label>
			</div>
			<button on:click={newTodo} class="btn btn buttonSubmit w-100">
				Přidat
			</button>
		</form>
	</div>
</div>
