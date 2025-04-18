<script>
export default {
  data() {
    return {
      todoInput: "",
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
          id: this.generateNewId(),
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
    },
    generateNewId() {
      const randomId = Math.random().toString(36).substring(2);
      const checkId = this.todos.find((todo) => todo.id === randomId);

      return !checkId ? randomId : this.generateNewId();
    },
  },
};
</script>

<template>
  <h1 class="text-uppercase fw-bold text-center">vue todo</h1>
  <div class="container">
    <section class="input-group my-3">
      <input
        type="text"
        class="form-control"
        v-model="todoInput"
        @keydown.enter="addTodo"
      />
      <button @click="addTodo" class="btn btn-secondary">Add</button>
    </section>
    <ul class="list-group">
      <li
        v-for="todo in todos"
        :key="todo.id"
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
  background-color: rgb(65, 65, 65);
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
  background-color: #999999;
  animation: lineThroughAnimation 0.3s ease-in-out forwards;
}

.btn-secondary {
  background-color: #2196f3;
  border-color: #2196f3;
}

.btn-secondary:hover {
  background-color: #1e83d6;
  border-color: #1e83d6;
}

.btn-success {
  background-color: #2196f3;
  border-color: #2196f3;
}

.btn-success:hover {
  background-color: #1c84da;
  border-color: #1c84da;
}

.btn-danger {
  background-color: #f50057;
  border-color: #f50057;
}

.btn-danger:hover {
  background-color: #e20150;
  border-color: #e20150;
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
    background-color: rgb(65, 65, 65);
  }
  100% {
    background-color: rgb(65, 65, 65);
  }
}
</style>
