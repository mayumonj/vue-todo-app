<script>
let id = 0

export default {
  data() {
    return {
      newTodo: '',
      todos: [
        { id: id++, text: 'Learn HTML' , editing: false},
        { id: id++, text: 'Learn JavaScript' , editing: false},
        { id: id++, text: 'Learn Vue' , editing: false}
      ],
      flag: false
    }
  },
  computed: {
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === '') {
        this.newTodo = ''
        return
      }
      this.todos.push({ id: id++, text: this.newTodo, editing: false })
      this.newTodo = ''
    },
    editTodo(todo) {
      this.todos.forEach((item) => {
        if (item.id === todo.id) {
          item.editing = true
        }
      })
    },
    doneEdit(todo) {
      this.todos.forEach((item) => {
        if (item.id === todo.id) {
          item.editing = false
        }
      })
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    }
  }
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <table>
    <tr>
      <th>Todo</th>
      <th></th>
      <th></th>
    </tr>
    <tr v-for="todo in todos" :key="todo.id">
      <td v-if="todo.editing"><input type="text" v-model="todo.text"></td>
      <td v-else>{{ todo.text }}</td>
      <td v-if="todo.editing"><button @click="doneEdit(todo)">done</button></td>
      <td v-else><button @click="editTodo(todo)">edit</button></td>
      <td><button @click="removeTodo(todo)">remove</button></td>
    </tr>
  </table>
</template>

<style>
#app {
  margin: 20px;
}
table {
  margin-top: 20px;
}
td {
  padding: 5px;
}
</style>
