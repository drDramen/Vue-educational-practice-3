<template>
  <label class="sort-complete">
    <input
      type="checkbox"
      :checked="sortDirection"
      @change="$emit('update:sortDirection', $event.target.checked)"
      :disabled="!isCompletedTasks"
    />
    <span v-if="sortDirection">Сначала выполненные</span>
    <span v-else>Сначала не выполненные</span>
  </label>
</template>

<script>
export default {
  name: 'SortbyCompleted',
  props: {
    sortDirection: Boolean,
    isCompletedTasks: Boolean,
  },
};
</script>

<style></style>
