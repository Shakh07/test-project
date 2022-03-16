<template>
  <div id="app" class="relative">
    <LiftShaft class="absolute left-10 top-0 right-0 bottom-0" />
    <div class="mt-2 flex flex-col-reverse">
      <Stages v-for="stage in stages" :key="stage.id" :stage="stage" />
    </div>
  </div>
</template>

<script>
import Stages from "./components/Stages.vue";
import LiftShaft from "./components/lift/LiftShaft.vue";
export default {
  name: "App",
  components: { Stages, LiftShaft },
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
