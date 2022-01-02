<template>
  <div :key="task.id" v-for="task in tasks">
    <Task :task="task" />
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
};
</script>

<style scoped></style>
