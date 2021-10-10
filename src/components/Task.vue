<template>
  <li>
    <p>
      {{ task.title }}
    </p>
    <!--    v-if="task.completed"-->
    <button @click="$emit('deleted', task.id)">Del</button>
    <input type="checkbox" :value="task.completed" @change="$emit('completed', task.id)" />
  </li>
</template>

<script>
export default {
  name: 'Task',
  props: {
    task: Object,
  },
};
</script>
