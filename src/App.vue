<script>
export default {
  data() {
    return {
      todoInput: "",
      idx: 0,
      todos: [],
    };
  },
  mounted() {
    if (localStorage.getItem("todos")) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  },
  watch: {
    todos: {
      handler(val) {
        localStorage.setItem("todos", JSON.stringify(val));
      },
      deep: true,
    },
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
        :class="{ bg: todo.completed }"
      >
        <p
          class="m-0"
          :class="{ completed: todo.completed, 'line-through': todo.completed }"
        >
          {{ todo.act }}
        </p>
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
p {
  font-size: 18px;
}

.bg {
  background-color: rgb(230, 230, 230);
  animation: bgAnimation 0.3s ease-in-out forwards;
}

.completed {
  position: relative;
}

.completed.line-through::after {
  content: "";
  position: absolute;
  width: 0%;
  height: 2px;
  left: 0;
  top: calc(50%);
  background-color: black;
  animation: lineThroughAnimation 0.3s ease-in-out forwards;
}

@keyframes lineThroughAnimation {
  0% {
    width: 0%;
  }
  100% {
    width: 100%;
  }
}

@keyframes bgAnimation {
  0% {
    background-color: white;
  }
  100% {
    background-color: rgb(230, 230, 230);
  }
}
</style>
