<template>
  <div class="todo-list">
    <div v-if="todos.length === 0" class="empty-state">
      No todos found. Add a new one!
    </div>
    <ul v-else>
      <TodoItem 
        v-for="todo in todos" 
        :key="todo.id" 
        :todo="todo"
        @toggle="$emit('toggle-todo', todo.id)"
        @delete="$emit('delete-todo', todo.id)"
        @edit="$emit('edit-todo', todo.id, $event)"
      />
    </ul>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'

export default {
  name: 'TodoList',
  components: {
    TodoItem
  },
  props: {
    todos: {
      type: Array,
      required: true
    }
  },
  emits: ['toggle-todo', 'delete-todo', 'edit-todo']
}
</script>

<style scoped>
.todo-list {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  overflow: hidden;
}

ul {
  list-style-type: none;
}

.empty-state {
  padding: 20px;
  text-align: center;
  color: #888;
}
</style>