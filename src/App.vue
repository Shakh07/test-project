<template>
  <div id="app" class="mt-2">
    <Stages v-for="stage in stages" :key="stage.id" :stage="stage" />
  </div>
</template>

<script>
import Stages from "./components/Stages.vue";
export default {
  name: "App",
  components: { Stages },
  data() {
    return {
      stages: [
        { id: 1, number: 1 },
        { id: 2, number: 2 },
        { id: 3, number: 3 },
        { id: 4, number: 4 },
        { id: 5, number: 5 },
      ],
    };
  },
};
</script>

<style></style>
