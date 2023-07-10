<script setup>
import { reactive, ref } from "vue";

let tasks = ref([
  // reactive({ id: 0, title: "go to town", completed: false }),
  // reactive({ id: 1, title: "do my homeworks", completed: false }),
]);

let completedTasks = ref([
  // reactive({ id: 0, title: "hello", completed: true }),
]);

// const names =

let title = ref("");
let tasksID = ref(0);

const addtask = () => {
  if (tasks.value) {
    // Tasks is an array
    // But tasks.value  >> return us to an object
    // So we can get length of object like this >>> keys(tasks).length
    // tasksID.value = tasks.value.length;
    tasksID.value++;
  } else {
    tasksID.value = 0;
    // console.log("No items in array!!");
  }
  // push - add item into end of the array
  tasks.value.push({
    id: tasksID.value,
    title: title.value,
    completed: false,
  });
  title.value = "";
};

const donetask = (gaveTask) => {
  gaveTask.completed = true;

  // Find the index of which is click the done button in UI.
  // let taskIndex = tasks.value.findIndex((task) => task.id === gaveTask.id);
  tasks.value = tasks.value.filter((task) => task !== gaveTask); // This solution randomely came  from nowhere
  //let completedTask = tasks.value[taskIndex]; // store that object into "completedTask" variable

  // push the object we get in to "completedTasks" array
  completedTasks.value.push(gaveTask);
  // completedTasks.value.push(reactive(completedTask));
  // tasks.value.splice(taskIndex, 1); // Remove it from current "task" array
};

const deleteTask = (id, state) => {
  let taskIndex;

  if (state === "uncompleted") {
    taskIndex = tasks.value.findIndex((task) => task.id === id);
    tasks.value.splice(taskIndex, 1);
  } else if (state === "completed") {
    taskIndex = completedTasks.value.findIndex((task) => task.id === id);
    completedTasks.value.splice(taskIndex, 1);
  }
};
</script>

<template>
  <div class="text-left flex flex-col gap-4">
    <h3>add task :</h3>
    <form @submit.prevent="submitform" class="flex gap-4">
      <input
        type="text"
        name="title"
        placeholder="title of the task..."
        class="px-4 py-2 bg-slate-700 rounded focus:outline-none w-full"
        v-model="title" />
      <button
        class="bg-blue-600 transition-all hover:bg-blue-500 rounded"
        :class="{ 'bg-gray-500 hover:bg-gray-500': !title }"
        @click="addtask"
        :disabled="!title">
        <svg
          width="46"
          height="46"
          fill="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
          class="scale-75">
          <path
            d="M19 11h-6V5a1 1 0 0 0-2 0v6H5a1 1 0 0 0 0 2h6v6a1 1 0 0 0 2 0v-6h6a1 1 0 0 0 0-2Z"></path>
        </svg>
      </button>
      <button
        class="text-red-100 transition-all bg-red-500 hover:bg-red-600 rounded"
        @click="title = ''">
        <svg
          width="46"
          height="46"
          fill="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
          class="scale-75">
          <path
            d="M12 2a10 10 0 1 0 0 20 10 10 0 0 0 0-20Zm8 10a7.92 7.92 0 0 1-1.69 4.9L7.1 5.69A7.92 7.92 0 0 1 12 4a8 8 0 0 1 8 8ZM4 12a7.92 7.92 0 0 1 1.69-4.9L16.9 18.31A7.92 7.92 0 0 1 12 20a8 8 0 0 1-8-8Z"></path>
        </svg>
      </button>
    </form>
    <h3
      class="mt-4 w-full px-4 py-4 bg-gray-700 bg-opacity-30 rounded-md"
      v-if="tasks.length">
      want to complete
    </h3>
    <h3
      class="mt-4 w-full px-4 py-4 bg-gray-700 bg-opacity-30 rounded-md"
      v-else>
      no tasks to complete
    </h3>
    <div class="flex flex-col gap-2">
      <div
        class="flex gap-4 items-center justify-between"
        v-for="task in tasks"
        v-motion
        :initial="{ opacity: 0, x: 100 }"
        :enter="{ opacity: 1, x: 0, scale: 1 }"
        :hovered="{ scale: 1.05 }"
        :delay="100">
        <p
          class="bg-slate-700 px-4 w-full max-w-xs text-ellipsis overflow-hidden py-3 rounded">
          {{ task.title }}
        </p>
        <span class="flex gap-2">
          <button
            class="text-green-100 transition-all bg-green-500 hover:bg-green-600 rounded"
            @click="donetask(task)"
            :disabled="task.completed"
            :class="{ 'text-gray-500 hover:bg-gray-100': task.completed }">
            <svg
              width="46"
              height="46"
              fill="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
              class="scale-75">
              <path
                d="M9.86 17.997a1.002 1.002 0 0 1-.73-.32l-4.86-5.17a1.001 1.001 0 0 1 1.46-1.37l4.12 4.39 8.41-9.2a1 1 0 1 1 1.48 1.34l-9.14 10a1.002 1.002 0 0 1-.73.33h-.01Z"></path>
            </svg>
          </button>
          <button
            class="text-red-100 transition-all bg-red-500 hover:bg-red-600 rounded"
            @click="deleteTask(task.id, 'uncompleted')">
            <svg
              width="46"
              height="46"
              fill="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
              class="scale-75">
              <path
                d="M21 6.001h-5v-1.67a2.42 2.42 0 0 0-2.5-2.33h-3A2.42 2.42 0 0 0 8 4.331v1.67H3a1 1 0 0 0 0 2h1v11a3 3 0 0 0 3 3h10a3 3 0 0 0 3-3v-11h1a1 1 0 1 0 0-2Zm-11-1.67c0-.16.21-.33.5-.33h3c.29 0 .5.17.5.33v1.67h-4v-1.67Zm8 14.67a1 1 0 0 1-1 1H7a1 1 0 0 1-1-1v-11h12v11Z"></path>
              <path
                d="M9 17a1 1 0 0 0 1-1v-4a1 1 0 1 0-2 0v4a1 1 0 0 0 1 1Z"></path>
              <path
                d="M15 17a1 1 0 0 0 1-1v-4a1 1 0 0 0-2 0v4a1 1 0 0 0 1 1Z"></path>
            </svg>
          </button>
        </span>
      </div>
    </div>
    <h3
      class="mt-4 flex gap-4 w-full px-4 py-2 bg-red-700 bg-opacity-10 rounded-md"
      v-if="tasks.length">
      <svg
        width="25"
        height="25"
        fill="currentColor"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg">
        <path
          d="M12 2a10 10 0 1 0 0 20 10 10 0 0 0 0-20Zm0 18a8 8 0 1 1 0-16.001A8 8 0 0 1 12 20Z"></path>
        <path d="M12 17a1 1 0 1 0 0-2 1 1 0 0 0 0 2Z"></path>
        <path d="M12 7a1 1 0 0 0-1 1v5a1 1 0 0 0 2 0V8a1 1 0 0 0-1-1Z"></path>
      </svg>
      {{ tasks.length }} - tasks to Complete Bud
    </h3>
    <h3
      class="w-full px-4 py-4 bg-gray-700 bg-opacity-30 rounded-md mt-4"
      v-if="completedTasks.length">
      Completed tasks
    </h3>
    <div class="flex flex-col gap-2 pb-32">
      <div
        class="flex gap-4 items-center justify-between"
        v-for="task in completedTasks"
        v-motion-slide-bottom>
        <h2
          class="bg-gray-700 line-through px-4 w-full max-w-xs text-ellipsis overflow-hidden py-3 rounded">
          {{ task.title }}
        </h2>
        <span class="flex gap-2">
          <button class="bg-gray-700 rounded" disabled>
            <svg
              width="46"
              height="46"
              fill="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
              class="scale-75">
              <path
                d="M9.86 17.997a1.002 1.002 0 0 1-.73-.32l-4.86-5.17a1.001 1.001 0 0 1 1.46-1.37l4.12 4.39 8.41-9.2a1 1 0 1 1 1.48 1.34l-9.14 10a1.002 1.002 0 0 1-.73.33h-.01Z"></path>
            </svg>
          </button>
          <button
            class="text-red-100 transition-all bg-red-500 hover:bg-red-600 rounded"
            @click="deleteTask(task.id, 'completed')">
            <svg
              width="46"
              height="46"
              fill="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
              class="scale-75">
              <path
                d="M21 6.001h-5v-1.67a2.42 2.42 0 0 0-2.5-2.33h-3A2.42 2.42 0 0 0 8 4.331v1.67H3a1 1 0 0 0 0 2h1v11a3 3 0 0 0 3 3h10a3 3 0 0 0 3-3v-11h1a1 1 0 1 0 0-2Zm-11-1.67c0-.16.21-.33.5-.33h3c.29 0 .5.17.5.33v1.67h-4v-1.67Zm8 14.67a1 1 0 0 1-1 1H7a1 1 0 0 1-1-1v-11h12v11Z"></path>
              <path
                d="M9 17a1 1 0 0 0 1-1v-4a1 1 0 1 0-2 0v4a1 1 0 0 0 1 1Z"></path>
              <path
                d="M15 17a1 1 0 0 0 1-1v-4a1 1 0 0 0-2 0v4a1 1 0 0 0 1 1Z"></path>
            </svg>
          </button>
        </span>
      </div>
      <button
        class="mt-4 w-full px-4 py-4 bg-red-700 bg-opacity-30 rounded-md hover:bg-opacity-50"
        v-if="completedTasks.length"
        @click="completedTasks = []"
        v-motion-slide-bottom>
        Clear All
      </button>
    </div>
  </div>
</template>
