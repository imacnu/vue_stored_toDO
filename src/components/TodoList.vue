<template>
  <div class="todo">
    <h2>{{ title }}</h2>
    <todo-form  v-on:add-todo="addTodo"></todo-form>
    <section>
      <article v-if="todos.length">
        <h4 class="subtitle">In progress</h4>
        <ul>
          <transition-group name="list">
            <li v-for="(todo, index) in todos" :key="index">
              <todo-list-item
                :todo="todo"
                v-on:remove-todo="removeTodo"
              ></todo-list-item>
            </li>
          </transition-group>
        </ul>
      </article>
      <article v-if="completedTodos.length">
        <h4 class="subtitle">Completed</h4>
        <ul>
          <transition-group name="list">
            <li v-for="(todo, index) in completedTodos" :key="index" :class="{completed: todo.completed}">
              <todo-list-item
                :todo="todo"
                v-on:remove-todo="removeCompletedTodo"
              ></todo-list-item>
            </li>
          </transition-group>
        </ul>
      </article>
    </section>
  </div>
</template>

<script>
import TodoForm from '@/components/TodoForm';
import TodoListItem from '@/components/TodoListItem';

export default {
  components: {TodoListItem, TodoForm},
  name: 'TodoList',
  data () {
    return {
      title: 'Todo List App',
      todos: [],
      completedTodos: [],
    }
  },
  methods: {
    addTodo(todo) {
      this.todos.push({id: this.todos.length, name: todo, completed: false});
    },
    removeTodo(todo) {
      this._remove(this.todos, todo);
      this.completedTodos.push({id: todo.id, name: todo.name, completed: true});
    },
    removeCompletedTodo(todo) {
      this._remove(this.completedTodos, todo);
    },
    _remove(collection, element) {
      collection.splice(collection.indexOf(element) , 1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2, h3, h4 {
  font-weight: normal;
}
section {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: space-between;
}
ul {
  list-style-type: none;
  padding: 0;

}
.subtitle {
  font-style: italic;
}
li {
  display: flex;
  width: 70%;
  margin: 0 auto;
}
a {
  color: #42b983;
}
.completed {
  text-decoration: line-through;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
