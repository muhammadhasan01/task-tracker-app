<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { Itask } from "@/utils/interfaces";

import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default defineComponent({
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [] as Itask[],
    };
  },
  methods: {
    async addTask(task: Itask): Promise<void> {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id: number): Promise<void> {
      if (confirm("Are you sure you?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });
        if (res.status === 200) {
          this.tasks = this.tasks.filter((task: Itask) => task.id !== id);
        } else {
          alert("Error deleting task");
        }
      }
    },
    async toggleReminder(id: number): Promise<void> {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };
      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task: Itask) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async fetchTasks(): Promise<void> {
      const res = await fetch("api/tasks");
      return await res.json();
    },
    async fetchTask(id: number): Promise<void> {
      const res = await fetch(`api/tasks/${id}`);
      return await res.json();
    },
  },
  async created(): Promise<void> {
    this.tasks = ref<Itask[]>(await this.fetchTasks());
  },
});
</script>
