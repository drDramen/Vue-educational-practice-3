<template>
  <div id="app">
    <div class="container">
      <todo-list />
    </div>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  components: {
    TodoList,
  },
  data: () => ({}),
};
</script>

<style lang="sass"></style>
