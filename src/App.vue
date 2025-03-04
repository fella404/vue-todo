<script>
export default {
  data() {
    return {
      todoInput: "",
      idx: 0,
      todos: [],
    };
  },
  methods: {
    addTodo() {
      if (this.todoInput !== "") {
        this.todos.push({
          id: this.idx++,
          act: this.todoInput,
          completed: false,
        });
        this.todoInput = "";
      }
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter((t) => t.id !== todo.id);
    },
    isCompleted(t) {
      for (const todo of this.todos) {
        if (todo.id === t.id) {
          todo.completed = !todo.completed;
        }
      }
      console.log(this.todos);
    },
  },
};
</script>

<template>
  <h1 class="text-success text-uppercase fw-bold text-center">vue todo</h1>
  <div class="container">
    <section class="input-group my-3">
      <input
        type="text"
        class="form-control"
        v-model="todoInput"
        @keydown.enter="addTodo"
      />
      <button @click="addTodo" class="btn btn-outline-secondary">Add</button>
    </section>
    <ul class="list-group" v-for="todo in todos" :key="todo.id">
      <li
        class="d-flex justify-content-between align-items-center list-group-item"
      >
        <p class="m-0" :class="{ completed: todo.completed }">{{ todo.act }}</p>
        <div class="btn-group">
          <button class="btn btn-danger me-1" @click="deleteTodo(todo)">
            Delete
          </button>
          <button class="btn btn-success" @click="isCompleted(todo)">
            {{ todo.completed ? "&#10005;" : "&#10003;" }}
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style>
.completed {
  text-decoration: line-through;
}
</style>
