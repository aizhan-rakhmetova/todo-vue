<template>
  <footer class="todo-footer">
    <span class="todo-count">
      <strong>{{ remaining }}</strong> {{ remaining === 1 ? 'item' : 'items' }} left
    </span>
    <div class="filters">
      <button 
        @click="$emit('set-filter', 'all')" 
        :class="{ active: filter === 'all' }"
        class="filter-btn"
      >
        All
      </button>
      <button 
        @click="$emit('set-filter', 'active')" 
        :class="{ active: filter === 'active' }"
        class="filter-btn"
      >
        Active
      </button>
      <button 
        @click="$emit('set-filter', 'completed')" 
        :class="{ active: filter === 'completed' }"
        class="filter-btn"
      >
        Completed
      </button>
    </div>
    <button 
      @click="$emit('clear-completed')" 
      class="clear-btn"
      v-if="remaining < totalTodos"
    >
      Clear completed
    </button>
  </footer>
</template>

<script>
import { computed } from 'vue'

export default {
  name: 'TodoFooter',
  props: {
    remaining: {
      type: Number,
      required: true
    },
    filter: {
      type: String,
      required: true
    }
  },
  emits: ['set-filter', 'clear-completed'],
  setup(props) {
    const totalTodos = computed(() => {
      return props.remaining + (props.filter === 'active' ? 0 : 1)
    })

    return {
      totalTodos
    }
  }
}
</script>

<style scoped>
.todo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  font-size: 0.9rem;
  color: #777;
}

.filters {
  display: flex;
  gap: 8px;
}

.filter-btn {
  padding: 3px 7px;
  border-radius: 3px;
  color: #777;
}

.filter-btn.active {
  border: 1px solid rgba(175, 47, 47, 0.2);
}

.filter-btn:hover {
  border: 1px solid rgba(175, 47, 47, 0.1);
}

.clear-btn {
  color: #777;
  text-decoration: none;
}

.clear-btn:hover {
  text-decoration: underline;
}
</style>