<template>
  <div
    class="border-r-2 border-l-2 border-t border-gray-400 w-32 flex flex-col-reverse"
  >
    <div
      class="bg-blue-600 h-32 transition-all ease-linear"
      :style="cabinPosition"
      :class="{ 'cabin-stop': isAnimate }"
    >
      <div v-if="isMoving" class="flex justify-center">
        <div
          class="flex justify-center bg-gray-400 w-1/2 opacity-70 rounded-sm"
        >
          <arrow-up v-if="isMovingUp" />
          <arrow-down v-else />
          <p class="text-white">{{ stage }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ArrowDown from "../icons/ArrowDown.vue";
import ArrowUp from "../icons/ArrowUp.vue";
export default {
  components: { ArrowUp, ArrowDown },
  props: {
    callStage: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      //кол-вызовов из разных этажей
      stackStages: [],
      stage: this.callStage,
      //Движение лифта
      isMoving: false,
      //направление движения
      isMovingUp: false,
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
    callStage(newStage) {
      if (!this.stackStages.includes(newStage)) {
        this.stackStages.push(newStage);
        this.moveToStage();
      }
    },
  },
  mounted() {
    if (this.getFromLocalStorage("stackStages")) {
      this.stackStages = this.getFromLocalStorage("stackStages");
      this.moveToStage();
    }
  },
  methods: {
    moveToStage() {
      //если нет очереди вызовов или лифт не движется
      if (!this.stackStages.length || this.isMoving || this.isAnimate) {
        return;
      }
      //Добавление вызовов и этажа в LocalStorage
      this.addToLocalstorage("stackStages", this.stackStages);
      //удаление из очереди
      const nextStage = this.stackStages.shift();
      //таймер движения этажа
      this.timer = Math.abs(this.stage - nextStage);
      this.isMovingUp = nextStage > this.stage; //направление движения
      this.stage = nextStage;
      this.addToLocalstorage("stage", this.stage);
      this.isMoving = true;
      setTimeout(() => {
        this.isMoving = false;
        this.isAnimate = true;
        //событие прибытия лифта
        this.$emit("arrived", nextStage);
        //После 3 секунды отдыха
        setTimeout(() => {
          this.isAnimate = false;
          this.moveToStage();
        }, 3000);
      }, this.timer * 1000);
    },
    addToLocalstorage(name, item) {
      localStorage.setItem(name, JSON.stringify(item));
    },
    getFromLocalStorage(name) {
      return JSON.parse(localStorage.getItem(name));
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
