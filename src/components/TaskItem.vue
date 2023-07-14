<template>
  <div class="TaskItem">
    <p class="TaskItem__taskName">
      {{ task.name }}
    </p>
    <p class="TaskItem__secondaryInfo">
      Due Date: {{ task.dueDate.toLocaleDateString() }}
    </p>
    <p class="TaskItem__secondaryInfo" v-if="task.location">
      Location: {{ task.location }}
    </p>
    <p class="TaskItem__secondaryInfo" v-if="task.location">
      category: {{ task.category }}
    </p>
  </div>
</template>

<script>
export default {
  name: "TaskItem",
  props: {
    task: Object,
  },
}
</script>

<style scoped lang="scss">

.TaskItem {
  padding: 5px 0 5px 10px;

  &__taskName {
     font-weight: bold;
   }
  &__secondaryInfo {
     font-size: 12px;
   }
}
</style>