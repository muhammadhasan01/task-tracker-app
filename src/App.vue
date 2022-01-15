<template>
  <div class="container">
    <Header
      @toggle-add-task="toggleAddTask"
      title="Task Tracker"
      :showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { ref } from "vue";
import { Itask } from "@/utils/interfaces";

import Header from "@/components/Header.vue";
import Tasks from "@/components/Tasks.vue";
import AddTask from "@/components/AddTask.vue";

@Options({
  name: "App",
  components: {
    Tasks,
    Header,
    AddTask,
  },
  data() {
    return {
      tasks: [] as Itask[],
      showAddTask: false,
    };
  },
  methods: {
    addTask(task: Itask) {
      this.tasks.push(task);
    },
    deleteTask(id: number): void {
      if (confirm("Are you sure you?")) {
        this.tasks = this.tasks.filter((task: Itask) => task.id !== id);
      }
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    toggleReminder(id: number): void {
      this.tasks = this.tasks.map((task: Itask) =>
        task.id === id ? { ...task, reminder: !task.reminder } : task
      );
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      return await res.json();
    },
    async fetchTask(id: number) {
      const res = await fetch(`api/tasks/${id}`)
      return await res.json();
    }
  },
  async created() {
    this.tasks = ref<Itask[]>(await this.fetchTasks());
  },
})
export default class App extends Vue {}
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
