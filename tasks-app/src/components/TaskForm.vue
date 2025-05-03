<script lang="ts" setup>
import { ref } from "vue";

const newTask = ref("");
const error = ref("");

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

function formSubmitter() {
  if (newTask.value.trim()) {
    emit("addTask", newTask.value.trim());
    newTask.value = "";
  } else {
    error.value = "Task cannot be empty!";
  }
}
</script>

<template>
  <form action="" v-on:submit.prevent="formSubmitter">
    <label for="newTask">
      New Task
      <input
        id="newTask"
        v-model="newTask"
        :aria-invalid="!!error || undefined"
        name="newTask"
        @input="error = ''"
      />
      <small v-if="error" id="invalid-helper"> {{ error }} </small>
    </label>
    <div class="button-container">
      <button>Add</button>
    </div>
  </form>
</template>
