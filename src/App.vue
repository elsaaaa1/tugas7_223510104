<template>
  <div class="container">
    <h1>BELAJAR PINIA</h1>
    <p>Bersama Ellsya🥰</p>

    <h2>Masukan List Tugas!!</h2>
    <div class="input-group">
      <input v-model="newTodo" placeholder="Isi Cepat!" />
      <button @click="addTodo">Add</button>
    </div>
    <p>Jumlah tugas kamu yang belum selesai😏: <strong>{{ incompleteTodosCount }}</strong></p>
    <transition-group name="list" tag="ul" class="todo-list">
      <li v-for="(todo, index) in todos" :key="todo.text" :class="{ completed: todo.completed }">
        <input type="checkbox" :checked="todo.completed" @change="toggleTodoCompletion(index)" />
        <span>{{ todo.text }}</span>
        <button @click="removeTodo(index)">Delete</button>
      </li>
    </transition-group>
    <p>Semangat Nugasnya🦍💪</p>
    <p>ADA ORANGTUA YANG HARUS DIBAHAGIAKAN👩‍👩‍👦‍👦</p>
  </div>
</template>

<script>
import { defineStore, createPinia } from 'pinia'
import { ref, defineComponent, computed } from 'vue'

// Store untuk count
const useCountStore = defineStore('countStore', {
  state: () => ({
    count: 0
  }),
  getters: {
    doubleCount: (state) => state.count * 2
  },
  actions: {
    increment() {
      this.count++
    }
  }
})

// Store untuk todo list
const useTodoStore = defineStore('todoStore', {
  state: () => ({
    todos: []
  }),
  getters: {
    incompleteTodosCount: (state) => state.todos.filter(todo => !todo.completed).length
  },
  actions: {
    addTodo(todo) {
      this.todos.push(todo)
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
    toggleTodoCompletion(index) {
      this.todos[index].completed = !this.todos[index].completed
    }
  }
})

// Pinia instance
const pinia = createPinia()

export default defineComponent({
  setup() {
    const countStore = useCountStore(pinia)
    const todoStore = useTodoStore(pinia)
    const newTodo = ref('')

    const addTodo = () => {
      if (newTodo.value.trim()) {
        todoStore.addTodo({
          text: newTodo.value,
          completed: false
        })
        newTodo.value = ''
      }
    }

    const removeTodo = (index) => {
      todoStore.removeTodo(index)
    }

    const toggleTodoCompletion = (index) => {
      todoStore.toggleTodoCompletion(index)
    }

    // Gunakan computed untuk mendapatkan jumlah tugas yang belum selesai
    const incompleteTodosCount = computed(() => todoStore.incompleteTodosCount)

    return {
      countStore,
      newTodo,
      todos: todoStore.todos,
      addTodo,
      removeTodo,
      toggleTodoCompletion,
      incompleteTodosCount
    }
  }
})
</script>

<style scoped>
/* Container styling */
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  background-color: #ffe4e1; /* Soft pink background */
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

/* Header styling */
h1 {
  font-size: 2em;
  color: #333;
  margin-bottom: 20px;
}

h2 {
  font-size: 1.5em;
  margin: 20px 0;
  color: #ff69b4; /* Hot pink text */
}

/* Input group styling */
.input-group {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 10px;
  font-size: 1em;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 70%;
  margin-right: 10px;
}

button {
  padding: 10px 15px;
  font-size: 1em;
  background-color: #ff69b4; /* Hot pink button */
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #ff1493; /* Deep pink button on hover */
}

/* Todo list styling */
.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-list li {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 10px;
  margin: 10px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  transition: all 0.3s ease;
}

.todo-list li.completed span {
  text-decoration: line-through;
  color: grey;
}

.todo-list span {
  flex: 1;
  cursor: pointer;
}

.todo-list span:hover {
  text-decoration: underline;
}

/* Checkbox styling */
input[type="checkbox"] {
  margin-right: 10px;
  width: 20px;
  height: 20px;
  cursor: pointer;
}

/* Transition for adding/removing items */
.list-enter-active, .list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

/* Additional styling */
p {
  font-size: 1em;
  color: #333;
  margin: 10px 0;
}
</style>
