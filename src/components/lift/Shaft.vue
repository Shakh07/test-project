<template>
  <div class="border-2 border-gray-400 w-32 flex flex-col-reverse">
    <Cabin :style="cabinPosition" :class="{ 'cabin-stop': isCabinStoped }" />
  </div>
</template>

<script>
import Cabin from "./Cabin.vue";
export default {
  components: { Cabin },
  props: {
    callStage: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      isCabinStoped: false,
    };
  },
  computed: {
    cabinPosition() {
      return {
        "margin-bottom": 128 * this.callStage - 128 + "px",
      };
    },
  },
  watch: {
    callStage() {
      setTimeout(() => {
        this.isCabinStoped = true;
      }, 3000);
      this.isCabinStoped = false;
    },
  },
};
</script>

<style scoped>
.cabin-stop {
  animation-name: cabin;
  animation-iteration-count: 3;
  animation-timing-function: linear;
  animation-duration: 1.5s;
}
@keyframes cabin {
  from {
    opacity: 1;
  }
  to {
    opacity: 0.1;
  }
}
</style>
