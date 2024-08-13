<template>
  <div class="todo-item">
    <input 
      type="checkbox" 
      class="todo-item__checkbox outlined" 
      :class="{'todo-item__checkbox--checked': todo.checked}" 
      :checked="todo.checked"
      @click="changeChecked" 
    />
    <label 
      class="todo-item__label" 
      :class="{'todo-item__label--checked': todo.checked}"
      :for="`todo-${todo.id}`"
    > 
      {{ todo.title }}
    </label>
    <div 
      class="todo-item__delete"
      @click="removeItem"
    >
    </div>
  </div>
</template>

<script setup>
import {defineEmits, defineProps} from "vue";

defineProps(['todo'])
const emit = defineEmits(['changeChecked', 'removeItem'])

function changeChecked() {
  emit("changeChecked")
}

function removeItem() {
  emit("removeItem")
}
</script>