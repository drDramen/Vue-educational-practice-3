<template>
  <div class="todo-list__wrapper">
    <add-task class="block__wrapper" @createTask="addTask" />

    <search-tasks class="block__wrapper" v-model="taskSearch" />

    <task-list
      class="block__wrapper"
      v-bind:tasksList="currentShowTasks"
      @deleted="deleteTask"
      @completed="completeTask"
    />
  </div>
</template>

<script>
import TaskList from '@/components/TasksList';
import AddTask from '@/components/AddTask.vue';
import SearchTasks from '@/components/SearchTasks.vue';

export default {
  components: {
    TaskList,
    AddTask,
    SearchTasks,
  },
  data: function () {
    return {
      tasks: [],
      taskSearch: '',
      sortDirection: false,
      currentPage: 1,
      limitOnPage: 5,
    };
  },
  computed: {
    sortedTasks() {
      return [...this.tasks].sort((a) => (a.completed === this.sortDirection ? 1 : -1));
    },
    filterableTasks() {
      return this.sortedTasks.filter((t) => t.title.includes(this.taskSearch));
    },
    currentShowTasks() {
      const startIndex = (this.currentPage - 1) * this.limitOnPage;
      return this.filterableTasks.slice(startIndex, startIndex + this.limitOnPage);
    },
    // totalPages() {
    //   return Math.ceil(this.tasks.length / this.limit);
    // },
    // totalCompletedTasks() {
    //   return this.sortedTasks.filter((t) => t.completed).length;
    // },
  },
  beforeMount() {
    const tasks = localStorage.getItem('tasks');
    if (tasks) {
      this.tasks = JSON.parse(tasks);
    }
  },
  methods: {
    saveTasksToLS() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    addTask(task) {
      this.tasks.push(task);
      this.saveTasksToLS();
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((t) => t.id !== id);
      this.saveTasksToLS();
    },
    completeTask(id) {
      const taskIndex = this.tasks.findIndex((task) => task.id === id);
      this.tasks[taskIndex].completed = !this.tasks[taskIndex].completed;
      this.saveTasksToLS();
    },
  },
};
</script>

<style lang="sass">
.todo-list__wrapper
  padding: 50px 30px
.block__wrapper
  padding: 20px
</style>
