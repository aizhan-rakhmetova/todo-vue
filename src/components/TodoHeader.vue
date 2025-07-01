<template>
  <header class="header">
    <h1>Todo List</h1>
    <div class="input-container">
      <input 
        type="text" 
        v-model="newTodo" 
        @keyup.enter="addTodo"
        placeholder="What needs to be done?"
        class="todo-input"
      />
      <button @click="addTodo" class="btn btn-primary">Add</button>
    </div>
  </header>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'TodoHeader',
  emits: ['add-todo'],
  setup(props, { emit }) {
    const newTodo = ref('')

    const addTodo = () => {
      if (newTodo.value.trim()) {
        emit('add-todo', newTodo.value)
        newTodo.value = ''
      }
    }

    return {
      newTodo,
      addTodo
    }
  }
}
</script>

<style scoped>
.header {
  margin-bottom: 20px;
}

h1 {
  color: var(--primary-color);
  text-align: center;
  margin-bottom: 20px;
}

.input-container {
  display: flex;
  gap: 10px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  border: 1px solid var(--border-color);
  border-radius: 4px;
  font-size: 1rem;
}
</style>