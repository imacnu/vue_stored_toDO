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
                v-if="!editing"
                :todo="todo"
                v-on:remove-todo="removeTodo"
                v-on:edit-todo="editTodo"
              ></todo-list-item>
              <input class="edit" type="text"
                v-if="editing"
                v-model="todo.name"
                @focus="editTodo(todo)"
                @blur="doneEdit(todo)"
                @keyup.enter="doneEdit(todo)"
                @keyup.esc="cancelEditTodo(todo)">
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

const TODOS_STORE = 'todos-store';
const COMPLETED_TODOS_STORE = 'completed-todos-store';

export default {
  components: {TodoListItem, TodoForm},
  name: 'TodoList',

  data () {
    return {
      title: 'Todo List App',
      todos: [],
      completedTodos: [],
      editedTodo: false,
      beforeEditCache: '',
      editing: false
    }
  },

  created() {
    this.todos = JSON.parse(localStorage.getItem(TODOS_STORE) || []);
    this.completedTodos = JSON.parse(localStorage.getItem(COMPLETED_TODOS_STORE) || []);
  },

  methods: {

    addTodo(todo) {
      this.todos.push({id: this.todos.length, name: todo, completed: false});
      this._updateStore(TODOS_STORE, this.todos);
    },

    removeTodo(todo) {
      this._remove(this.todos, todo);
      this._updateStore(TODOS_STORE, this.todos);

      this.completedTodos.push({id: todo.id, name: todo.name, completed: true});
      this._updateStore(COMPLETED_TODOS_STORE, this.completedTodos);
    },

    removeCompletedTodo(todo) {
      this._remove(this.completedTodos, todo);
      this._updateStore(COMPLETED_TODOS_STORE, this.completedTodos);
    },

    _updateStore(storeKey, collection) {
      localStorage.setItem(storeKey, JSON.stringify(collection));
    },
    _remove(collection, element) {
      collection.splice(collection.indexOf(element) , 1);
    },

    editTodo(todo) {
      this.beforeEditCache = todo.name;
      this.editedTodo = todo;
      this.editing = true;
    },

    doneEdit: function (todo) {
      if (!this.editedTodo) {
        return
      }
      this._remove(this.todos, todo);
      if (todo.name) {
        this.todos.push(todo);
      }
      this._updateStore(TODOS_STORE, this.todos);
      this.editedTodo = null
      this.editing = false;
    },

    cancelEditTodo(todo) {
      this.editedTodo = null
      todo.name = this.beforeEditCache;
      this.editing = false;
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
.edit {
  display: block;
  border: none;
  border-bottom: 1px solid  #2c3e50;
  outline: none;
  text-align: left;
  font-size: 1rem;
  color: #2c3e50;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  opacity: .7;
  font-style: italic;
}
</style>
