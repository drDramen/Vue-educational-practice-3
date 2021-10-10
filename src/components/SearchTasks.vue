<template>
  <div class="task-search">
    <p>Поиск:</p>
    <input
      class="custom-input"
      type="text"
      :value="value"
      @input="$emit('input', $event.target.value)"
      placeholder="Искомый текст"
    />
  </div>
</template>

<script>
export default {
  name: 'SearchTasks',
  props: {
    value: String,
  },
};
</script>

<style lang="sass" scoped>
.task-search
  font-size: 3.2rem
  p
    margin-right: 20px
    padding: 0 34px
    text-transform: uppercase
    letter-spacing: 4px
</style>
