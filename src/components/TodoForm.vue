<template>
  <div>
    <form @submit.prevent="addTodo" @keyup.enter="addTodo">
      <input type="text" v-model="todo" placeholder="Add todo ...">
      <button>&#x271B;</button>
    </form>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        todo: ''
      }
    },
    methods: {
      addTodo(ev) {
        if(this.todo) {
          this.$emit('add-todo', this.todo);
          this.todo = '';
        }
      }
    },

  }
</script>

<style scoped>
  form {
    width: 80%;
    margin: 2rem auto;
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
  input {
    width: 90%;
    background-color: #42b983;
    padding: .8rem;
    border: none;
    border-radius: .1rem;
    box-shadow: 0 8px 6px -6px #34495e ;
    color: #ecf0f1;
    outline: none;
    font-size: 1rem;
  }

  ::placeholder {
    color: #ecf0f1;
    opacity: .8;
  }

  button {
    border: none;
    background: none;
    outline: none;
    font-size: 2rem;
    color: #42b983;;
  }
</style>