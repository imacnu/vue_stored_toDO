<template>
  <div class="item">
    <p>{{todo.name}}</p>
    <button @click="removeTodo"> &#x2715; </button>
  </div>
</template>

<script>
  export default {
    props: ['todo'],
    methods: {
      removeTodo() {
        this.$emit('remove-todo', this.todo);
      }
    },

  }
</script>

<style scoped>
  .item {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }
  button {
    background: none;
    border: none;
    font-size: 1rem;
    outline: none;
    color: tomato;
  }

</style>