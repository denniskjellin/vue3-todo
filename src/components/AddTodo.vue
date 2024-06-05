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
      <h2 v-if="todos.length > 0">My tasks</h2>
      <ul v-if="todos.length > 0">
        <li v-for="todo in todos" :key="todo.id" :class="{ 'done': todo.completed }">
          <div class="task-item">
            <input class="checkmark" type="checkbox" v-model="todo.completed" @change="toggleTaskCompletion(todo)">
            <span>{{ todo.text }}</span>
          </div>
          <button class="delete-button" @click="deleteTodo(todo.id)">Delete</button>
        </li>
      </ul>
    </div>
    <div v-if="message" class="message">{{ message }}</div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup() {
    let id = 0
    const newTodo = ref('') // Represents the input value for a new todo item
    const todos = ref([]) // Represents the list of todo items
    const message = ref('') // Represents the message to be displayed

    onMounted(() => {
      const savedTodos = JSON.parse(localStorage.getItem('todos'))
      if (savedTodos) {
        todos.value = savedTodos
      }
    })


    // Adds a new todo item to the list of todos
    function addTodo() {
      todos.value.push({ id: id++, text: newTodo.value, completed: false })
      newTodo.value = ''
      saveTodos()
      showMessage('Task added')
    }


    // Deletes a todo item from the list of todos
    function deleteTodo(todoId) {
      todos.value = todos.value.filter(todo => todo.id !== todoId)
      saveTodos()
      showMessage('Task deleted')
    }

    // Toggles the completion status of a todo item
    function toggleTaskCompletion(todo) {
      saveTodos()
    }


    // Saves the todos to local storage
    function saveTodos() {
      localStorage.setItem('todos', JSON.stringify(todos.value))
    }


    function showMessage(text) {
      message.value = text
      setTimeout(() => {
        message.value = ''
      }, 3000)
    }

    return {
      newTodo,
      addTodo,
      todos,
      deleteTodo,
      toggleTaskCompletion,
      message
    }
  }
}
</script>

<style>
.container {
  padding: 0 1rem;
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
  margin: 1.5rem 0;
  font-weight: bold;
  transition: ease-in-out 0.2s;
}

.add-button:hover {
  background-color: rgb(78, 165, 132);
}

.todo-container {
  margin-top: 3rem;
  width: 100%;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: rgba(95, 95, 95, 0.303);
  margin-bottom: 0.5rem;
  padding: 1rem;
  border-radius: 5px;
}

.done {
  color: rgb(149, 149, 149);
}

.done span {
  text-decoration: line-through;
}

span {
  margin-left: 1rem;
}

li:hover {
  background-color: rgba(82, 81, 81, 0.303);
}

.task-item {
  display: flex;
  align-items: center;
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

.checkmark {
  height: 1.2rem;
  width: 1.2rem;
  background-color: #eee;
}

.message {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  z-index: 999;
}
</style>
