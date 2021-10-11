<template>
  <div class="todo-list__wrapper">
    <div class="todo-list__header">
      <add-task class="block__wrapper flex" @createTask="addTask" />
      <search-tasks class="block__wrapper flex" v-model="taskSearch" />
    </div>
    <div class="tasks-list__header flex">
      <h2 class="tasks-list__title">Список задач</h2>
      <div v-if="totalPages > 1" class="tasks-list__nav tasks-list-nav flex">
        <p class="tasks-list-nav__btn" @click="prevPage">Назад</p>
        <p>страница {{ currentPage }} из {{ totalPages }}</p>
        <p class="tasks-list-nav__btn" @click="nextPage">Вперед</p>
      </div>
    </div>
    <div class="flex" style="flex-direction: column">
      <p>Выполнено {{ totalCompletedTasks }}</p>
      <sortby-completed :sortDirection.sync="sortDirection" v-bind:isCompletedTasks="isCompletedTasks" />
    </div>
    <task-list
      class="block__wrapper tasks-list__item"
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
import SortbyCompleted from '@/components/SortbyCompleted.vue';

export default {
  components: {
    TaskList,
    AddTask,
    SearchTasks,
    SortbyCompleted,
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
      return [...this.tasks].sort((a) => (a.completed === this.sortDirection ? -1 : 1));
    },
    filterableTasks() {
      return this.sortedTasks.filter((t) => t.title.includes(this.taskSearch));
    },
    currentShowTasks() {
      const startIndex = (this.currentPage - 1) * this.limitOnPage;
      return this.filterableTasks.slice(startIndex, startIndex + this.limitOnPage);
    },
    totalPages() {
      return Math.ceil(this.tasks.length / this.limitOnPage);
    },
    totalCompletedTasks() {
      return this.sortedTasks.filter((t) => t.completed).length;
    },
    isCompletedTasks() {
      return this.totalCompletedTasks > 0;
    },
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
      if (this.currentPage > this.totalPages) {
        this.currentPage--;
      }
      this.saveTasksToLS();
    },
    completeTask(id) {
      const taskIndex = this.tasks.findIndex((task) => task.id === id);
      this.tasks[taskIndex].completed = !this.tasks[taskIndex].completed;
      this.saveTasksToLS();
    },
    nextPage() {
      if (this.currentPage < this.totalPages) this.currentPage += 1;
    },
    prevPage() {
      if (this.currentPage > 1) this.currentPage -= 1;
    },
  },
};
</script>

<style lang="sass">
.todo-list
  &__wrapper
    padding: 50px 30px
  &__header
    margin-bottom: 50px

.block__wrapper
  padding: 20px 0

.tasks-list
  &__header
    justify-content: space-between
    margin-bottom: 30px
    height: 56px
    border-bottom: 2px solid #fff
  &__title
    position: relative
    display: inline-block
    font-size: 3.2rem
    text-transform: uppercase
  &__nav
    justify-content: flex-end
    align-items: baseline
    & > p + p
      margin-left: 20px
  &-nav__btn
    padding: 5px
    font-size: 2.4rem
    cursor: pointer
    &:hover
      text-decoration: underline

.tasks-list__item
  padding: 20px

.sort-complete
  align-self: flex-start
  input
    margin-right: 5px
</style>
