<script lang="ts">
  import { fly } from "svelte/transition";
  export let todo: any;
  export let remove: function;
  /* let color: any = todo.completed ? "is-success" : "is-info" */
</script>

<div
  class="notification"
  class:is-success={todo.completed}
  transition:fly={{ duration: 1000, x: 200 }}>
  <input type="checkbox" bind:checked={todo.completed} />
  {todo.title}
  <button class="delete" on:click={() => remove(todo)} />
</div>
