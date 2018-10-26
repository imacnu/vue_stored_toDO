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
    width: 10rem;
    display: flex;
    justify-content: space-between;
  }
  button {
    font-size: 1rem;
    color: tomato;
  }

</style>