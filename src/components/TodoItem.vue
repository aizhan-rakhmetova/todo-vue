<template>
  <li class="todo-item">
    <div v-if="!editing" class="todo-view">
      <div class="todo-content">
        <input 
          type="checkbox" 
          :checked="todo.completed" 
          @change="$emit('toggle')"
          class="todo-checkbox"
        />
        <span 
          @dblclick="startEditing" 
          :class="{ completed: todo.completed }"
          class="todo-text"
        >
          {{ todo.text }}
        </span>
      </div>
      <button @click="$emit('delete')" class="delete-btn">
        ×
      </button>
    </div>
    <div v-else class="todo-edit">
      <input 
        type="text" 
        v-model="editText" 
        @blur="finishEditing"
        @keyup.enter="finishEditing"
        @keyup.esc="cancelEditing"
        ref="editInput"
        class="edit-input"
      />
    </div>
  </li>
</template>

<script>
import { ref, nextTick } from 'vue'

export default {
  name: 'TodoItem',
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  emits: ['toggle', 'delete', 'edit'],
  setup(props, { emit }) {
    const editing = ref(false)
    const editText = ref('')
    const editInput = ref(null)

    const startEditing = () => {
      editText.value = props.todo.text
      editing.value = true
      nextTick(() => {
        editInput.value.focus()
      })
    }

    const finishEditing = () => {
      if (editing.value) {
        if (editText.value.trim()) {
          emit('edit', editText.value)
        }
        editing.value = false
      }
    }

    const cancelEditing = () => {
      editing.value = false
    }

    return {
      editing,
      editText,
      editInput,
      startEditing,
      finishEditing,
      cancelEditing
    }
  }
}
</script>

<style scoped>
.todo-item {
  border-bottom: 1px solid var(--border-color);
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-view {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
}

.todo-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex: 1;
}

.todo-checkbox {
  width: 20px;
  height: 20px;
  cursor: pointer;
}

.todo-text {
  word-break: break-word;
}

.delete-btn {
  color: var(--secondary-color);
  font-size: 1.5rem;
  opacity: 0.5;
  transition: opacity 0.3s;
}

.delete-btn:hover {
  opacity: 1;
}

.todo-edit {
  padding: 12px 16px;
}

.edit-input {
  width: 100%;
  padding: 8px;
  border: 1px solid var(--primary-color);
  border-radius: 4px;
  font-size: 1rem;
}
</style>