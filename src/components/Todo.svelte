<script>
	import { createEventDispatcher } from "svelte";

	export let todo;
	export let index;

	const dispatch = createEventDispatcher();

	function getDaysLeft(deadline) {
		if (!deadline) {
			return "-";
		}
		const now = new Date();
		const deadlineDate = new Date(deadline);
		const timeDiff = deadlineDate.getTime() - now.getTime();
		if(Math.ceil(timeDiff / (1000 * 60 * 60 * 24)) < 0) {
			return `Jsi pozadu ${Math.ceil(timeDiff / (-1000 * 60 * 60 * 24))} dnů`
		}
		else if(Math.ceil(timeDiff / (1000 * 60 * 60 * 24)) == 0) {
			return `Termín je dnes!`;
		}
		else {
		return `${Math.ceil(timeDiff / (1000 * 60 * 60 * 24))} dnů`;
		}
	}

	function removeFromList(index) {
		dispatch("removeTodo", index);
	}
</script>

<tr>
	<td class="text-muted">{index + 1}</td>
	<td class:checked={todo.status}>
		<strong>{todo.text}</strong>
	</td>
	<td class="phone">{todo.deadline || "-"}</td>
	<td class="phone">{getDaysLeft(todo.deadline)}</td>
	<td>
		{#if todo.status}
			dokončeno
		{:else}
			nedokončeno
		{/if}
	</td>
	<td>
		<button
			class="btn btn-danger me-2"
			on:click={() => removeFromList(index)}>Smazat</button
		>
		<div class="form-check form-switch d-inline-block">
			<input
				type="checkbox"
				class="form-check-input"
				id={"checkbox" + index}
				bind:checked={todo.status}
			/>
			<label class="form-check-label" for={"checkbox" + index}
				>Dokončit</label
			>
		</div>
	</td>
</tr>

<style>
	.checked {
		text-decoration: line-through;
	}
</style>
