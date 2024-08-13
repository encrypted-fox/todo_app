<template>
  <div class="todo-container">
    <form class="todo-form" v-on:submit.prevent="addNewTodo">
      <label class="todo-form__label" for="new-todo">Добавьте элемент</label>
      <input
        class="todo-form__input outlined"
        v-model="newTodoText"
        id="new-todo"
        placeholder="Напишите..."
      />
      <button class="todo-form__button outlined">Добавить</button>
    </form>

    <div class="todo-list" v-if="todos.length">
      <todo-item
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @changeChecked="onChangeChecked(todo.id)"
        @removeItem="onRemoveItem(todo.id)"
      ></todo-item>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import TodoItem from "./components/TodoItem.vue";
import { v4 as uuidv4 } from "uuid";

const newTodoText = ref("");
const todos = ref(JSON.parse(localStorage.getItem("todos")) || []);

function addNewTodo() {
  if (newTodoText.value) {
    todos.value.unshift({
      id: uuidv4(),
      title: newTodoText.value,
      checked: false,
    });
    newTodoText.value = "";

    localStorage.setItem("todos", JSON.stringify(todos.value));
  }
}

function onChangeChecked(todoId) {
  const todosIndex = todos.value.findIndex((el) => el.id === todoId);

  if (todosIndex !== -1) {
    const checkedTodo = todos.value.splice(todosIndex, 1)[0];
    if (checkedTodo.checked) {
      checkedTodo.checked = !checkedTodo.checked;
      todos.value.unshift(checkedTodo);
    } else {
      checkedTodo.checked = !checkedTodo.checked;
      todos.value.push(checkedTodo);
    }

    localStorage.setItem("todos", JSON.stringify(todos.value));
  }
}

function onRemoveItem(todoId) {
  const todosIndex = todos.value.findIndex((el) => el.id === todoId);

  if (todosIndex !== -1) {
    todos.value.splice(todosIndex, 1)[0];

    localStorage.setItem("todos", JSON.stringify(todos.value));
  }
}
</script>

<style>
#app {
  font-family: "Inter", sans-serif;
}
.todo-container {
  width: 50%;
  margin: 15px auto;
}
.todo-form {
  background: linear-gradient(0.25turn, #2e3192, #12b8b8);
  padding: 10px;
  border-radius: 5px;
}
.todo-form__label {
  color: white;
}

.todo-form__input {
  margin-left: 15px;
  padding: 5px 10px;
  border-radius: 5px;
  border: 2px solid transparent;
  height: 30px;
  box-sizing: border-box;
}

.todo-form__button {
  margin-left: 10px;
  height: 30px;
  border-radius: 5px;
  border: none;
  background: white;
  padding-right: 15px;
  padding-left: 15px;
  cursor: pointer;
  color: #2e3192;
}

.todo-form__button:hover {
  background: #cfe1ff;
  transition: background ease 0.2s;
}

.todo-form__button:active {
  background: #c2d9ff;
  transition: background ease 0.2s;
}

.todo-list {
  background: linear-gradient(0.25turn, #2e3192, #12b8b8);
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
  color: white;
}

.todo-item {
  margin-top: 10px;
  display: flex;
  width: 100%;
  gap: 10px;
  align-items: center;
}

.todo-item:nth-child(1) {
  margin: 0;
}

.todo-item__checkbox {
  border: none;
  width: 18px;
  height: 18px;
}

.todo-item__checkbox--checked {
  border: none;
  width: 18px;
  height: 18px;
  border-radius: 5px;
  filter: hue-rotate(-45deg);
}

.todo-item__label {
  max-width: calc(100% - 35px);
}

.todo-item__label--checked {
  color: #12b8b8;
  text-decoration: line-through;
}

.todo-item__delete:after {
  margin-left: auto;
  font-size: 20px;
  display: inline-block;
  content: "\00d7";
}

.outlined:focus {
  outline: 2px solid #c2d9ff;
  transition: outline ease 0.2s;
}
</style>
