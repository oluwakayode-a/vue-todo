<template>
  <div class="hello">
    <form @submit.prevent="addTodo">
      <input type="text" v-model="todo_input" placeholder="Enter a todo" v-validate="'min:5|max:10|required'" maxlength="11" name="todo">
      <button type="submit" class="submit">Add Todo</button>
    </form>
    <transition name="error" enter-active-class="animated headShake" leave-active-class="animated fadeOut">
      <p class="error" v-if="errors.has('todo')">{{ errors.first('todo') }}</p>
    </transition>

    <section class="todos-container">
      <transition-group enter-active-class="animated zoomIn" leave-active-class="animated zoomOut">
        <div class="todo-card" v-for="(todo, index) in todos" :key="index">
          <span class="delete" @click="deleteTodo(index)">x</span>
          <p class="todo-text">{{ todo.text }}</p>
          <button class="todo-btn complete" v-if="!todo.completed" @click="completeTodo(index)">Complete</button>
          <button class="todo-btn completed" v-else @click="completeTodo(index)"><img src="../assets/close.svg" alt="" class="completed-icon"></button>
        </div>
      </transition-group>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Todos',
  data() {
    return {
      todo_input: '',
      todos: [],
    }
  },
  methods: {
    addTodo() {
      // check if form is valid
      this.$validator.validateAll().then((result) => {
        if(result) {
          this.todos.push({text: this.todo_input, completed: false});
          // add todos to localStorage
          localStorage.setItem('todos', JSON.stringify(this.todos))
          // clear todo
          this.todo_input = '';
        } else {
            console.log('Invalid')
        }
      })
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
      // Delete from localStorage
      localStorage.setItem('todos', JSON.stringify(this.todos))
    },
    completeTodo(index) {
      this.todos[index].completed = true;
    }
  },
  mounted() {
    if (localStorage.getItem('todos') === null) {
        this.todos = []
    } else {
      this.todos = JSON.parse(localStorage.getItem('todos'))
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";

  input {
    padding: .75rem 7rem 1rem 1rem;
    border-radius: 10px;
    border: 1px solid #707070;
    font-family: 'Poppins';
  }

  .submit {
    margin-left: 10px;
    padding: 1rem;
    color: white;
    background: #7BB98E;
    border: none;
    border-radius: 10px;
  }

  .todos-container > span {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 20px;
  }

  .todo-card {
    border: 1px solid #707070;
    padding: 3rem;
    margin: 10px 10px;
    position: relative;
    height: auto;
    width: 100px;
  }

  .delete {
    position: absolute;
    top: 10px;
    right: 15px;
    font-weight: bolder;
    cursor: pointer;
  }

  button {
    border: 1px solid #707070;
    border-radius: 20px;
    cursor: pointer;
    padding: 10px 20px;
    font-family: 'Poppins';
    background: none;
  }

  .todo-btn {
    position: absolute;
    left: 15px;
    bottom: -1px;
  }

  .completed {
    background-color: #7BB98E;
    color: white;
    border: none;
  }

  .completed-icon {
    width: 40%;
  }
</style>
