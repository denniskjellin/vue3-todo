<template>
  <div class="container">
    <div class="form">
      <form @submit.prevent="addTodo">
        <label for="addTodo">Create a task</label>
        <input name="addTodo" id="addTodo" v-model="newTodo" required placeholder="Whats on your mind?" />
        <button class="add-button">Add Task</button>
      </form>
    </div>

    <div class="todo-container">
      <h2>My tasks</h2>
      <ul>
        <li v-for="todo in todos" :key="todo.id">
          {{ todo.text }}
          <button class="delete-button" @click="deleteTodo(todo.id)">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'AddTodo',
  setup() {
    let id = 0
    const newTodo = ref('')
    const todos = ref([
      { id: id++, text: 'Learn Vue.js' },
      { id: id++, text: 'Build a todo app' },
      { id: id++, text: 'Practice JavaScript' }
    ])

    function addTodo() {
      todos.value.push({ id: id++, text: newTodo.value })
      newTodo.value = ''
    }

    function deleteTodo(todoId) {
      todos.value = todos.value.filter(todo => todo.id !== todoId)
    }

    return {
      newTodo,
      addTodo,
      todos,
      deleteTodo
    }
  }
}
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

form {
  background-color: rgba(95, 95, 95, 0.303);
  padding: 3rem 2rem 2rem 2rem;
  border-radius: 15px;
}

input {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: none;
  border-radius: 4px;
  box-sizing: border-box;
  background-color: rgb(207, 212, 216)
}

label {
  font-size: 1.5rem;
  font-weight: bold;
}

.add-button {
  background-color: rgb(27, 119, 84);
  color: white;
  padding: 1rem 2rem;
  border-radius: 25px;
  border: none;
  margin: 1.5rem;
    font-weight: bold;
    transition: ease-in-out 0.2s;
  }
  
  .add-button:hover {
    background-color: rgb(78, 165, 132);
  }
  
  .todo-container {
    margin-top: 3rem;
    width: 500px;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  li {
    background-color: rgba(95, 95, 95, 0.303);
    margin-bottom: 0.5rem;
    padding: 1rem;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  li:hover {
    background-color: rgba(82, 81, 81, 0.303);
  }
.delete-button {
  background-color: #800c17;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  padding: 0.5rem 1rem;
  font-size: 1rem;
}

.delete-button:hover {
  background-color: #660a13;
}
</style>
