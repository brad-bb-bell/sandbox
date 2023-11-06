<template>
  <main class="px-8">
    <h1 class="text-center">Home</h1>

    <h2 class="py-4">Names</h2>
    <draggable v-model="list1" itemKey="id" tag="ol" group="people" @end="saveOrder">
      <template #item="{ element }">
        <li class="cursor-pointer">{{ element.name }}</li>
      </template>
    </draggable>

    <h2 class="py-4">Jobs</h2>
    <draggable v-model="list2" itemKey="id" tag="ol" group="people" @end="saveOrder">
      <template #item="{ element }">
        <li class="cursor-pointer">{{ element.name }}</li>
      </template>
    </draggable>
  </main>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable,
  },
  data() {
    return {
      list1: [
        { name: "John", id: 1 },
        { name: "Joao", id: 2 },
        { name: "Jean", id: 3 },
        { name: "Gerard", id: 4 },
      ],
      list2: [
        { name: "Developer", id: 1 },
        { name: "Designer", id: 2 },
        { name: "Manager", id: 3 },
        { name: "Tester", id: 4 },
      ],
    };
  },
  methods: {
    saveOrder() {
      console.log("saveOrder");
    },
  },
};
</script>
