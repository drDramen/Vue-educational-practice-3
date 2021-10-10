<template>
  <div class="task-create">
    <button class="task-create__btn" :disabled="isEmpty" @click="createTask">Добавить</button>
    <input
      class="custom-input"
      ref="input"
      type="text"
      v-model="taskTitle"
      @keydown.enter="createTask"
      placeholder="Введите название задачи"
    />
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
.task-create
  //justify-content: space-between
  font-size: 3.2rem
  &__btn
    margin-right: 20px
    padding: 10px 20px
    text-transform: uppercase
</style>
