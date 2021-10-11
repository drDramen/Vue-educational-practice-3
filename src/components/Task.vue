<template>
  <li class="task flex">
    <input class="task__complete" type="checkbox" :checked="task.completed" @change="$emit('completed', task.id)" />
    <p class="task__title">
      {{ task.title }}
    </p>
    <!--    v-if="task.completed"-->
    <button class="task__btn-del" @click="$emit('deleted', task.id)"></button>
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

<style lang="sass" scoped>
.task
  margin-bottom: 30px
  align-items: center
  font-size: 3.2rem
  &__title
    flex-grow: 1
    margin-right: 20px
  &__complete
    flex-shrink: 0
    margin-right: 20px
    width: 50px
    height: 50px
  &__btn-del
    width: 50px
    height: 50px
    background-image: url(../assets/svg/delete.svg)
    background-repeat: no-repeat
    background-position: center
</style>
