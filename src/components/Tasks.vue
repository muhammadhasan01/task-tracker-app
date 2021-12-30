<template>
  <div :key="task.id" v-for="task in tasks">
    <h3>{{ task.text }}</h3>
  </div>
</template>

<script lang="ts">
import {PropType} from "vue";
import {Itask} from "@/utils/interfaces";

export default {
  name: "Tasks",
  props: {
    tasks: Array as PropType<Itask[]>
  }
}
</script>

<style scoped>

</style>
