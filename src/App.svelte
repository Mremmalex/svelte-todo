<script lang="ts">
  import Todo from "./components/Todo.svelte";
  import { onMount } from "svelte";
  import axios from "axios";

  $: pendings = todos.filter((t) => !t.completed);
  $: completed = todos.filter((t) => t.completed);
  let loading: boolean = false;
  interface Todo {
    userId?: number;
    id: number;
    title: string;
    completed: boolean;
  }
  let input: string = "";
  let todos: Array<any> = [];

  onMount(async () => {
    loading = true;
    const reponse = await axios.get(
      "https://jsonplaceholder.typicode.com/todos"
    );
    todos = reponse.data;
    loading = false;
  });

  function add(): void {
    const todo: Todo = {
      id: todos.length + 1,
      title: input,
      completed: false,
    };
    todos = [todo, ...todos];
    input = "";
  }

  function remove(todo: any): void {
    todos = todos.filter((t) => t.id !== todo.id);
  }
</script>

<div class="section">
  <div class="field has-addons">
    <input type="text" class="input" bind:value={input} />
    <button class="button is-success" on:click={add}>Add Todos</button>
  </div>
</div>
{#if loading}
  <div class="section">
    <p>loading.....</p>
  </div>
{:else if todos.length > 0}
  <div class="section columns">
    <div class="column">
      {#each pendings as todo (todo.id)}
        <Todo bind:todo {remove} />
      {/each}
    </div>
    <div class="column">
      {#each completed as todo (todo.id)}
        <Todo bind:todo {remove} />
      {/each}
    </div>
  </div>
{:else}
  <div class="section">
    <p>to add another todo please use the input box above</p>
  </div>
{/if}
