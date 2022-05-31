<template>
  <ul class="list-group">
    <li class="list-group-item list-group-item-action p-4" style="cursor: pointer" v-for="(task, index) in tasks"
      :key="index" @click="router.push(`/tasks/${task._id}`)">
      {{ index + 1 }}.
      {{ task.title }}
    </li>
  </ul>
</template>

<script lang="ts">
import { Task } from "@/interfaces/Task";
import { getTasks } from "@/services/TaskService";
import { defineComponent, reactive, onMounted, ref } from "vue";
import { useRouter } from "vue-router";
export default defineComponent({
  name: "tasks-list",
  setup() {
    const router = useRouter();
    let tsk = reactive([] as Task[]);
    let tasks = ref(tsk)

    async function loadTasks() {
      try {
        const res = await getTasks();
        tasks.value = res.data;
        console.log(tasks)
      } catch (error) {
        console.error(error);
      }
    }
    onMounted(() => {
      loadTasks();
    })
    return { tasks, router, loadTasks }
  }
  /*  data() {
     return {
       tasks: [] as Task[],
     };
   },
   methods: {
     async loadTasks() {
       try {
         const res = await getTasks();
         this.tasks = res.data;
       } catch (error) {
         console.error(error);
       }
     },
   },
   mounted() {
     this.loadTasks();
   }, */
});
</script>