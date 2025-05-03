<script lang="ts" setup>
import type { Task } from "../types.ts";
import { watch } from "vue";

const props = defineProps<{
  tasks: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
}>();

watch(
  () => props.tasks,
  (newVal) => {
    console.log("Tasks changed:", [...newVal]);
  },
  { deep: true },
);

function handleRemove(id: string) {
  console.log("Emitting removeTask with id:", id);
  emits("removeTask", id);
}
</script>

<template>
  <TransitionGroup class="task-list" name="task-list" tag="div">
    <article v-for="task in props.tasks" :key="task.id" class="task">
      <label for="checkBox">
        <input
          :checked="task.done"
          name="checkBox"
          type="checkbox"
          @input="emits('toggleDone', task.id)"
        />
        <span :class="{ done: task.done }">
          {{ task.title }}
        </span>
      </label>
      <button class="outline" @click="handleRemove(task.id)">Remove</button>
    </article>
  </TransitionGroup>
</template>

<style>
.task-list {
  margin-top: 1rem;
}

.done {
  text-decoration: line-through;
}

.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-list-enter-active,
.task-list-leave-active {
  transition: all 0.5s ease;
}

.task-list-enter-from,
.task-list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
