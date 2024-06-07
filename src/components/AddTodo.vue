<template>
  <div class="container">
    <div class="form">
      <form @submit.prevent="addTodo">
        <label for="addTodo">Create a task</label>
        <input name="addTodo" id="addTodo" v-model="newTodo" required placeholder="What's on your mind?" />
        <button class="add-btn">Add Task</button>
      </form>
    </div>

    <div class="todo-container">
      <h2 v-if="todos.length > 0">My tasks</h2>
      <button v-if="todos.length > 0" class="sort-btn" @click="sortByDate">{{ asc ? 'Sort by oldest' : 'Sort by newest' }}</button>

      <div v-if="todos.length > 0">
        <div v-for="todo in todos" :key="todo.id" :class="{ 'done': todo.completed }" class="todo-item">
          <div class="todo-content">
            <input class="checkmark" type="checkbox" v-model="todo.completed" @change="toggleTodoCompletion(todo)">
            <span class="todo-text">{{ todo.text }}</span>
            <div class="delete-btn-container">
              <button class="delete-btn" @click="deleteTodo(todo.id)">Delete</button>
            </div>
          </div>
          <div class="date-container">
            <span class="date">Created at: {{ todo.createdAt }}</span>
          </div>
        </div>
      </div>
    </div>
    <div v-if="message" class="trigger-message">{{ message }}</div>
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
    let asc = ref(true) // State variable to track sorting order

    onMounted(() => {
      const savedTodos = JSON.parse(localStorage.getItem('todos'))
      if (savedTodos) {
        todos.value = savedTodos
      }
    })


    // Adds a new todo item to the list of todos
    function addTodo() {
      // Date options for the locale
      const dateOptions = { year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit' };
      const locale = 'sv-SE';

      todos.value.push({
        id: id++,
        text: newTodo.value,
        completed: false,
        createdAt: new Date().toLocaleString(locale, dateOptions)
      });
      newTodo.value = '';
      saveTodos();
      showMessage('Task added');
    }

    // Sorts the todos by date
    function sortByDate() {
      if (todos.value.length === 0) {
        return;
      }
      todos.value = todos.value.sort((a, b) => {
        return asc.value ? (a.createdAt > b.createdAt ? 1 : -1) : (a.createdAt < b.createdAt ? 1 : -1);
      });
      asc.value = !asc.value;
    }

    // Deletes a todo item from the list of todos
    function deleteTodo(todoId) {
      todos.value = todos.value.filter(todo => todo.id !== todoId)
      saveTodos()
      showMessage('Task deleted')
    }

    // Toggles the completion status of a todo item
    function toggleTodoCompletion(todo) {
      const completed = todo.completed;
      saveTodos();
      if (completed) {
        showMessage('Task completed');
      } else {
        showMessage('Task marked as incomplete');
      }
    }

    // Saves the todos to local storage
    function saveTodos() {
      localStorage.setItem('todos', JSON.stringify(todos.value))
    }

    // Displays a message for a short period of time
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
      toggleTodoCompletion,
      message,
      sortByDate,
      asc
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
  background-color: rgb(207, 212, 216);
}

label {
  font-size: 1.5rem;
  font-weight: bold;
}

.add-btn {
  background-color: rgb(27, 119, 84);
  color: white;
  padding: 1rem 2rem;
  border-radius: 25px;
  border: none;
  margin: 1.5rem 0;
  font-weight: bold;
  transition: ease-in-out 0.2s;
}

.add-btn:hover {
  background-color: rgb(78, 165, 132);
}

.todo-container {
  margin-top: 3rem;
  width: 100%;
}

.todo-item {
  background-color: rgba(95, 95, 95, 0.303);
  margin-bottom: 0.5rem;
  padding: 1rem;
  border-radius: 5px;
}

.done {
  color: rgb(149, 149, 149);
  background-color: rgba(96, 96, 96, 0.222);
}

.done .todo-text {
  text-decoration: line-through;
}


.todo-content {
  display: flex;
  align-items: center;
}

.todo-content span {
  margin-left: 1rem;
}

.date-container {
  text-align: right;
  margin-top: 0.5rem;
}

.date {
  font-size: 0.9rem;
  color: #c0c0c0;
}

.delete-btn-container {
  margin-left: auto;
}

.delete-btn {
  background-color: #800c17;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  padding: 0.5rem 1rem;
}

.delete-btn:hover {
  background-color: #660a13;
}

.checkmark {
  height: 1.2rem;
  width: 1.2rem;
  background-color: #eee;
}

.trigger-message {
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

.sort-btn {
  background-color: rgba(95, 95, 95, 0.303);
  color: #42b983;
  border: none;
  border-radius: 5px 5px 0 0;
  cursor: pointer;
  transition: background-color 0.3s ease;
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
}
</style>
