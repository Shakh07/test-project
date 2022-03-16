<template>
  <div class="border-2 border-gray-400 w-32 flex flex-col-reverse">
    <div
      class="bg-blue-600 h-32 transition-all ease-linear"
      :style="cabinPosition"
      :class="{ 'cabin-stop': isAnimate }"
    ></div>
  </div>
</template>

<script>
export default {
  props: {
    callStage: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      //кол-вызовов ии разных этажей
      stackStages: [],
      stage: this.callStage,
      //Движение лифта
      isMoving: false,
      //Моргание лифта
      isAnimate: false,
      timer: 1,
    };
  },
  computed: {
    cabinPosition() {
      return {
        "margin-bottom": 128 * this.stage - 128 + "px",
        "transition-duration": this.timer + "s",
      };
    },
  },
  watch: {
    callStage(stage) {
      if (!this.stackStages.includes(stage)) {
        this.stackStages.push(stage);
        this.moveToStage();
      }
    },
  },
  methods: {
    moveToStage() {
      //если нет очереди вызовов или лифт не движется
      if (!this.stackStages.length || this.isMoving) {
        return;
      }
      //удаление из очереди
      const currentStage = this.stackStages.shift();
      //таймер движения этажа
      this.timer = Math.abs(this.stage - currentStage);
      this.stage = currentStage;
      this.isMoving = true;
      this.isAnimate = false;

      setTimeout(() => {
        this.isMoving = false;
        this.isAnimate = true;

        setTimeout(() => {
          this.moveToStage();
        }, 3000);
      }, this.timer * 1000);
    },
  },
};
</script>

<style scoped>
.cabin-stop {
  animation-name: cabin;
  animation-iteration-count: 3;
  animation-timing-function: linear;
  animation-duration: 1s;
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
