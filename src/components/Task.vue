<template>
  <div
    @dblclick="$emit('toggle-reminder', task.id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <h3>
      {{ task.text }}
      <i class="fas fa-times" @click="$emit('delete-task', task.id)" />
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Itask } from "@/utils/interfaces";

export default defineComponent({
  name: "Task",
  props: {
    task: Object as () => Itask,
  },
});
</script>

<style scoped>
.fas {
  color: red;
}

.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.reminder {
  border-left: 5px solid green;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
