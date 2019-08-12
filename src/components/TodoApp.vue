<template>
  <div class="todoWrapper">
    <div class="formInput">
      <input
        class="todoInput"
        placeholder="What needs to be done?"
        v-model="newTodo"
        @keyup.enter="addTodo"
      />
    </div>
    <ul>
      <li v-for="todo of todos" :key="todo.id">
        <div class="round">
          <input type="checkbox" :id="todo.id" v-model="todo.completed" />
          <label :for="todo.id"></label>
        </div>
        <div class="labelContent">
          <del class="todoLabel" v-if="todo.completed">
            <div>{{ todo.title }}</div>
          </del>
          <div v-else>{{ todo.title }}</div>
        </div>
        <button @click="handleTodoDelete(todo)" class="closeBtn">x</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { value, onMounted, watch } from 'vue-function-api'

export default {
  setup () {
    // reactive state
    const newTodo = value('')
    const todos = value([
      // { id: "1", title: "Buy milk", completed: false },
      // { id: "2", title: "Wash car", completed: true },
      // { id: "3", title: "Pay bill", completed: false }
    ])
    // methods
    const handleTodoDelete = todo => {
      todos.value = todos.value.filter(t => t !== todo)
    }
    const addTodo = () => {
      if (newTodo.value) {
        todos.value.unshift({
          title: newTodo.value,
          completed: false,
          id: Date.now().toString()
        })
        newTodo.value = ''
      }
    }
    onMounted(() => {
      if (localStorage.savedTodos) {
        todos.value.push(...JSON.parse(localStorage.savedTodos))
      }
      watch(
        () => todos.value,
        newTodos => {
          localStorage.savedTodos = JSON.stringify(newTodos)
        },
        { deep: true }
      )
    })
    return {
      newTodo,
      todos,
      handleTodoDelete,
      addTodo
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  -webkit-padding-start: 0px;
  -moz-padding-start: 0px;
  margin-bottom: 0;
}
ul li {
  list-style-type: none;
  min-height: 60px;
  max-height: 60px;
  /* overflow: auto; */
  display: flex;
  font-size: 14px;
  align-items: center;
  border-bottom: 1px solid #ededed;
  background-color: #fff;
  position: relative;
}
.closeBtn {
  background-color: transparent;
  border: none;
  color: #cc9a9a;
  position: absolute;
  right: 15px;
  padding: 0;
  font-size: 25px;
}
.closeBtn i {
  font-size: 20px;
}
.closeBtn:hover {
  color: #af5b5e;
}
.closeBtn:focus {
  outline: none;
}
.labelContent {
  display: block;
  padding-left: 1px;
  color: #777;
  width: calc(100% - 128px);
  max-height: 60px;
  overflow: auto;
  align-items: center;
}
.round {
  position: relative;
  width: 28px;
  height: 28px;
  margin: 0 15px;
}
.round label {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 50%;
  cursor: pointer;
  height: 28px;
  left: 0;
  position: absolute;
  top: 0;
  width: 28px;
  min-width: 28px;
}
.round label:after {
  border: 2px solid #66bb6a;
  border-top: none;
  border-right: none;
  content: "";
  height: 6px;
  left: 7px;
  opacity: 0;
  position: absolute;
  top: 8px;
  transform: rotate(-45deg);
  width: 12px;
}
.round input[type="checkbox"] {
  visibility: hidden;
}
.round input[type="checkbox"]:checked + label {
  background-color: #fff;
  border-color: #66bb6a;
}
.round input[type="checkbox"]:checked + label:after {
  opacity: 1;
}
.formInput {
  position: relative;
  display: flex;
  align-items: center;
}
.formInput input {
  padding: 16px 16px 16px 60px;
  border: none;
  background-color: #fff;
  width: 100%;
  -webkit-box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.1);
  -moz-box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.1);
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.1);
  opacity: 0.8;
}
.formInput input:focus {
  outline: none;
}
</style>
