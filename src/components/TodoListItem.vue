<template>
  <div class="item">
    <p>{{todo.name | capitalize}}</p>
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
    filters: {
      capitalize(name) {
        let _name = name.toLowerCase();
        return _name.charAt(0).toUpperCase() + _name.slice(1)
      }
    }
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