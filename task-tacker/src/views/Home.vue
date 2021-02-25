<template>
  <div>
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

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";
import axios from "axios";

export default {
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
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      try {
        const { data } = await axios.post("api/tasks", task);
        this.tasks = [...this.tasks, data];
      } catch (error) {
        console.log(error);
      }
    },
    async deleteTask(id) {
      if (confirm("Are you sure you want to delete?")) {
        const res = await fetch(`api/tasks/${id}`, { method: "DELETE" });
        res.status === 200
          ? (this.tasks = this.tasks.filter((t) => t.id !== id))
          : alert("Error deleting task");
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTasks(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };
      const { data } = await axios.put(`api/tasks/${id}`, updTask);
      this.tasks = this.tasks.map((t) =>
        t.id === id ? { ...t, reminder: data.reminder } : t
      );
      return data;
    },
    async fetchTask() {
      try {
        const { data } = await axios.get("api/tasks");
        return data;
      } catch (error) {
        console.log(error);
      }
    },
    async fetchTasks(id) {
      try {
        const { data } = await axios.get(`api/tasks/${id}`);
        return data;
      } catch (error) {
        console.log(error);
      }
    },
  },
  async created() {
    this.tasks = await this.fetchTask();
  },
};
</script>
