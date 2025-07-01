<template>
  <div class="container">
    <TodoHeader @add-todo="addTodo" />
    <TodoList 
      :todos="filteredTodos" 
      @toggle-todo="toggleTodo" 
      @delete-todo="deleteTodo"
      @edit-todo="editTodo"
    />
    <TodoFooter 
      :remaining="remaining" 
      :filter="filter"
      @set-filter="setFilter"
      @clear-completed="clearCompleted"
    />
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import axios from 'axios'
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoList,
    TodoFooter
  },
  setup() {
    const todos = ref([])
    const filter = ref('all')
    const loading = ref(false)
    const error = ref(null)

    const fetchTodos = async () => {
      loading.value = true
      error.value = null
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
        todos.value = response.data.map(todo => ({
          id: todo.id,
          text: todo.title,
          completed: todo.completed
        }))
      } catch (err) {
        error.value = 'Failed to fetch todos'
        console.error(err)
      } finally {
        loading.value = false
      }
    }

    const addTodo = async (text) => {
      if (!text.trim()) return
      
      const newTodo = {
        userId: 1,
        title: text,
        completed: false
      }
      
      try {
        const response = await axios.post('https://jsonplaceholder.typicode.com/todos', newTodo)
        todos.value.push({
          id: response.data.id,
          text: response.data.title,
          completed: response.data.completed
        })
      } catch (err) {
        error.value = 'Failed to add todo'
        console.error(err)
      }
    }

    const toggleTodo = async (id) => {
      const todo = todos.value.find(todo => todo.id === id)
      if (!todo) return
      
      const updatedTodo = { ...todo, completed: !todo.completed }
      
      try {
        await axios.put(`https://jsonplaceholder.typicode.com/todos/${id}`, {
          ...updatedTodo,
          title: updatedTodo.text
        })
        todo.completed = !todo.completed
      } catch (err) {
        error.value = 'Failed to update todo'
        console.error(err)
      }
    }

    const deleteTodo = async (id) => {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        todos.value = todos.value.filter(todo => todo.id !== id)
      } catch (err) {
        error.value = 'Failed to delete todo'
        console.error(err)
      }
    }

    const editTodo = async (id, newText) => {
      const todo = todos.value.find(todo => todo.id === id)
      if (!todo || !newText.trim()) return
      
      try {
        await axios.put(`https://jsonplaceholder.typicode.com/todos/${id}`, {
          ...todo,
          title: newText
        })
        todo.text = newText
      } catch (err) {
        error.value = 'Failed to edit todo'
        console.error(err)
      }
    }

    const clearCompleted = () => {
      const completedIds = todos.value
        .filter(todo => todo.completed)
        .map(todo => todo.id)

      Promise.all(
        completedIds.map(id => 
          axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        )
      )
        .then(() => {
          todos.value = todos.value.filter(todo => !todo.completed)
        })
        .catch(err => {
          error.value = 'Failed to clear completed todos'
          console.error(err)
        })
    }

    const setFilter = (newFilter) => {
      filter.value = newFilter
    }

    const filteredTodos = computed(() => {
      switch (filter.value) {
        case 'active':
          return todos.value.filter(todo => !todo.completed)
        case 'completed':
          return todos.value.filter(todo => todo.completed)
        default:
          return todos.value
      }
    })

    const remaining = computed(() => {
      return todos.value.filter(todo => !todo.completed).length
    })

    onMounted(fetchTodos)

    return {
      todos,
      filter,
      loading,
      error,
      filteredTodos,
      remaining,
      addTodo,
      toggleTodo,
      deleteTodo,
      editTodo,
      clearCompleted,
      setFilter
    }
  }
}
</script>