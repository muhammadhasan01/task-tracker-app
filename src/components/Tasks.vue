<template>
  <div :key="task.id" v-for="task in tasks">
    <Task
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @delete-task="$emit('delete-task', task.id)"
      :task="task"
    />
  </div>
</template>

<script lang="ts">
import { PropType } from "vue";
import { Itask } from "@/utils/interfaces";

import Task from "@/components/Task.vue";

export default {
  name: "Tasks",
  props: {
    tasks: Array as PropType<Itask[]>,
  },
  components: {
    Task,
  },
  emits: ["delete-task", "toggle-reminder"],
};
</script>

<style scoped></style>
