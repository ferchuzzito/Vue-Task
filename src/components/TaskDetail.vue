<template>
  <div class="col-md-4 offset-md-4">
    <form @submit.prevent="handleUpdate()" class="card card-body">
      <h1 class="card-title my-3 text-center">Update a Task</h1>

      <input type="text" v-model="currentTask.title" class="form-control mb-3" />

      <textarea v-model="currentTask.description" class="form-control mb-3"></textarea>

      <button class="btn btn-primary">Update</button>
    </form>

    <button @click="handleDelete()" class="btn btn-danger my-4">delete</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, onMounted, ref } from "vue";
import {useRoute, useRouter} from "vue-router";
import { Task } from "@/interfaces/Task";
import { deleteTask, getTask, updateTask } from "@/services/TaskService";
export default defineComponent({
  name: "task-Detail",
  setup() {
    const route = useRoute();
    const router = useRouter()
    let cTask = reactive({} as Task);
    let currentTask = ref(cTask);

    async function loadTask(id: string) {
      try {
        const { data } = await getTask(id);
        currentTask.value = data;
      } catch (error) {
        console.error(error);
      }
    }
    async function handleUpdate() {
      try {
        if (typeof route.params.id === "string") {
          await updateTask(route.params.id, currentTask.value);
          router.push("/");
        }
      } catch (error) {
        console.error(error);
      }
    }
    async function handleDelete() {
      try {
        if (typeof route.params.id === "string") {
          deleteTask(route.params.id);
          router.push("/");
        }
      } catch (error) {
        console.error(error);
      }
    }
    onMounted(() => {
      if (typeof route.params.id === "string") {
        loadTask(route.params.id);
      }
    })
    return { currentTask, loadTask, handleUpdate, handleDelete }
  }

  /*
    data() {
      return {
        currentTask: {} as Task,
      };
    },
    methods: {
      async loadTask(id: string) {
        try {
          const { data } = await getTask(id);
          this.currentTask = data;
        } catch (error) {
          console.error(error);
        }
      },
      async handleUpdate() {
        try {
          if (typeof this.$route.params.id === "string") {
            await updateTask(this.$route.params.id, this.currentTask);
            this.$router.push("/");
          }
        } catch (error) {
          console.error(error);
        }
      },
      async handleDelete() {
        try {
          if (typeof this.$route.params.id === "string") {
            deleteTask(this.$route.params.id);
            this.$router.push("/");
          }
        } catch (error) {
          console.error(error);
        }
      },
    },
    mounted() {
      if (typeof this.$route.params.id === "string") {
        this.loadTask(this.$route.params.id);
      }
    }, */
});

</script>