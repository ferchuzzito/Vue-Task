<template>
  <div class="col-md-4 offset-4">
    <div class="card card-body">
      <h1 class="card-title my-3 text-center">Create a Task</h1>
      <form @submit.prevent="saveTask()">
        <input class="form-control mb-3" type="text" placeholder="Escribe un titulo" v-model="task.title" />
        <textarea class="form-control mb-3" rows="3" placeholder="Escribe una descripcion"
          v-model="task.description"></textarea>
        <button class="btn btn-primary w-100" :disabled="!task.title || !task.description">
          Save
        </button>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from "vue";
import { useRouter } from "vue-router";
import { Task } from "@/interfaces/Task";
import { createTask } from "@/services/TaskService";

export default defineComponent({
  setup() {
    const router = useRouter();
    const task = reactive({} as Task);

    async function saveTask() {
      try {
        const res = await createTask(task);
        console.log(res);
        router.push({ name: "tasks" });
      } catch (error) {
        console.error(error);
      }
    }
    return { task, saveTask }
  }
  /*   data() {
      return {
        task: {} as Task,
      };
    },
    methods: {
      async saveTask() {
        try {
          const res = await createTask(this.task);
          console.log(res);
          this.$router.push({ name: "tasks" });
        } catch (error) {
          console.error(error);
        }
      },
    }, */
});
</script>

