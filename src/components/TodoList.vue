<script setup>
import { ref } from 'vue'

const todoItems = ref([])
const text = ref('')

const todoBeingEditedId = ref('')

function addTodo(newTodoText) {
  if (!newTodoText || newTodoText.length <= 3) return

  todoItems.value = [
    ...todoItems.value,
    {
      id: todoItems.value.length + 1,
      text: newTodoText,
      isCompleted: false
    }
  ]

  text.value = ''
}

function deleteTodo(obj) {
  todoItems.value = todoItems.value.filter((item) => item.id !== obj.id)
}

function handleEditTodo(item) {
  // Asign todoBeingEditedId
  todoBeingEditedId.value = item.id

  const currentTodoBeingEdited = todoItems.value.find((todoItem) => todoItem.id === item.id)

  text.value = currentTodoBeingEdited.text
}

function saveTodo(updatedText) {
  todoItems.value = todoItems.value.map((todoItem) => {
    if (todoItem.id === todoBeingEditedId.value) {
      return { ...todoItem, text: updatedText }
    }

    return todoItem
  })

  text.value = ''
  todoBeingEditedId.value = ''
}

function cancelEditingTodo() {
  todoBeingEditedId.value = ''
  text.value = ''
}
</script>

<template>
  <form @submit.prevent="!todoBeingEditedId ? addTodo(text) : saveTodo(text)">
    <input type="text" placeholder="Type your todo here" v-model.lazy.trim="text" />
    <button type="submit">
      {{ todoBeingEditedId ? 'Save Todo' : 'Add Todo' }}
    </button>
    <template v-if="todoBeingEditedId">
      <button @click="cancelEditingTodo">Cancel</button></template
    >
  </form>

  <ul>
    <template v-for="item of todoItems" :key="item.id">
      <li>
        <p>
          {{ item.text }}
        </p>

        <div>
          <button @click="handleEditTodo(item)">Edit</button>
          <button @click="deleteTodo(item)">Delete</button>
        </div>
      </li>
    </template>
  </ul>
</template>
