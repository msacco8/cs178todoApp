<script>
  import { afterUpdate } from 'svelte';

  let todoItems = [];
  let newTodo = '';
  let motivationalPhrases = ['You got this!', 'I believe in you!', 'Seize the day!'];

  function addTodo() {
    newTodo = newTodo.trim();
    if (!newTodo) return;

    // #Component
    const todo = {
        text: newTodo,
        checked: false,
        id: todoItems.length,
    };

    todoItems = [...todoItems, todo];
    newTodo = '';
  }

  function toggleDone(id) {
    const index = todoItems.findIndex(item => item.id === Number(id));
    todoItems[index].checked = !todoItems[index].checked;
  }

  function deleteTodo(id) {
    todoItems = todoItems.filter(item => item.id !== Number(id));
  }

  // #Reactive
  afterUpdate(() => {
    document.querySelector('.js-todo-input').focus();
    // #ControlFlow
    if (todoItems.length > 0) {
      document.querySelector('.motivation').innerHTML = motivationalPhrases[Math.floor(Math.random() * motivationalPhrases.length)]
    }
  });

</script>

<!-- #Component -->
<main>
  <div class="container">
    <h1 class="app-title">todos</h1>
    <p class="motivation"></p>
    <ul class="todo-list">
      <!-- #ControlFlow -->
      {#each todoItems as todo (todo.id)}
        <!-- #Component -->
        <li class="todo-item {todo.checked ? 'done' : ''}">
          <input id={todo.id} type="checkbox" />
          <!-- #Reactive and #Properties and #EventHandler -->
          <label for={todo.id} class="tick" on:click={() => toggleDone(todo.id)}></label>
          <!-- #Properties -->
          <span>{todo.text}</span>
          <!-- #Reactive and #Properties and #EventHandler -->
          <button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
            <svg><use href="#delete-icon"></use></svg>
          </button>
        </li>
      <!-- #ControlFlow -->
      {/each}
    </ul>    
    <div class="empty-state">
      <svg class="checklist-icon"><use href="#checklist-icon"></use></svg>
      <h2 class="empty-state__title">Add your first todo</h2>
      <p class="empty-state__description">What do you want to get done today?</p>
    </div>
    <!-- #Reactive and #EventHandler -->
    <form on:submit|preventDefault={addTodo}>
      <input class="js-todo-input" type="text" aria-label="Enter a new todo item" placeholder="E.g. Build a web app" bind:value={newTodo}>
    </form>
  </div>
</main>
