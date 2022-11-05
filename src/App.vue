<script>
let id = 0;

export default {
  data() {
    return {
      newTodo: "",
      todos: [],
    };
  },
  computed: {},
  mounted() {
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.todos);
    } else {
      this.todos = [
        { id: id++, text: "Learn HTML", editing: false },
        { id: id++, text: "Learn JavaScript", editing: false },
        { id: id++, text: "Learn Vue", editing: false },
      ];
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === "") {
        this.newTodo = "";
        return;
      }
      this.todos.push({ id: id++, text: this.newTodo, editing: false });
      localStorage.setItem("todos", JSON.stringify(this.todos));
      this.newTodo = "";
    },
    editTodo(todo) {
      this.todos.forEach((item) => {
        if (item.id === todo.id) {
          item.editing = true;
        }
      });
    },
    doneEdit(todo) {
      this.todos.forEach((item) => {
        if (item.id === todo.id) {
          item.editing = false;
        }
      });
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo);
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    clearAllData() {
      localStorage.clear();
      this.todos = [];
    },
    setSampleData() {
      localStorage.clear();
      this.todos = [
        { id: id++, text: "Learn HTML", editing: false },
        { id: id++, text: "Learn JavaScript", editing: false },
        { id: id++, text: "Learn Vue", editing: false },
      ];
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
};
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" />
    <button>Add Todo</button>
  </form>
  <table>
    <tr>
      <th>Todo</th>
      <th></th>
      <th></th>
    </tr>
    <tr v-for="todo in todos" :key="todo.id">
      <td v-if="todo.editing"><input type="text" v-model="todo.text" /></td>
      <td v-else>{{ todo.text }}</td>
      <td v-if="todo.editing"><button @click="doneEdit(todo)">done</button></td>
      <td v-else><button @click="editTodo(todo)">edit</button></td>
      <td v-if="!todo.editing">
        <button @click="removeTodo(todo)">remove</button>
      </td>
    </tr>
  </table>
  <div class="dev">
    <button @click="clearAllData()">Clear all data</button><br />
    <button @click="setSampleData()">Set sample data</button>
  </div>
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
.dev {
  margin-top: 100px;
}
</style>
