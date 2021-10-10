<template>
  <div class="task-create">
    <input class="task-create__title" ref="input" type="text" v-model="taskTitle" @keydown.enter="createTask" />
    <button :disabled="isEmpty" @click="createTask">Add</button>
  </div>
</template>

<script>
export default {
  name: 'AddTask',
  data: () => ({
    taskTitle: '',
  }),
  methods: {
    createTask() {
      if (!this.taskTitle) return;
      const task = {
        id: +new Date(),
        title: this.taskTitle,
        completed: false,
      };
      this.$emit('createTask', task);
      this.taskTitle = '';
      this.taskSearch = '';
      this.$refs.input.focus();
    },
  },
  computed: {
    isEmpty() {
      return this.taskTitle === '';
    },
  },
};
</script>

<style lang="sass">
.task-create__title
  background-color: transparent
  border: none
  border-bottom: 2px solid #fff
</style>
