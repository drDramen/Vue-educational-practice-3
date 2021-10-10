<template>
  <ul>
    <task
      v-for="task in tasksList"
      :key="task.id"
      v-bind:task="task"
      @deleted="$emit('deleted', $event)"
      @completed="$emit('completed', $event)"
    />
  </ul>
</template>

<script>
import Task from '@/components/Task';

export default {
  name: 'TaskList',
  components: {
    Task,
  },
  props: {
    tasksList: Array,
  },
};
</script>

<style lang="sass" scoped>
ul
  list-style: none
</style>
