<template>
  <div id="app" class="relative">
    <div class="absolute left-10 top-0 right-0 bottom-0 flex gap-x-4">
      <Shaft
        v-for="shaft in shafts"
        :key="shaft"
        class="relative"
        :callStage="callStage"
        @arrived="arrivedLift"
      />
    </div>
    <div class="mt-2 flex flex-col-reverse">
      <Stage
        v-for="stage in stages"
        :key="stage.id"
        :stage="stage"
        :shafts="shafts"
        @stage="getStage"
      />
    </div>
  </div>
</template>

<script>
import Stage from "./components/Stage.vue";
import Shaft from "./components/lift/Shaft.vue";
export default {
  name: "App",
  components: { Stage, Shaft },
  data() {
    return {
      stages: [
        { id: 1, number: 1, isClicked: false },
        { id: 2, number: 2, isClicked: false },
        { id: 3, number: 3, isClicked: false },
        { id: 4, number: 4, isClicked: false },
        { id: 5, number: 5, isClicked: false },
        { id: 6, number: 6, isClicked: false },
        { id: 7, number: 7, isClicked: false },
      ],
      shafts: [1, 2, 3, 4],
      callStage: 1,
    };
  },
  mounted() {
    this.checkLiftCallButtonState();
  },
  methods: {
    getStage(val) {
      this.callStage = val;
      this.stages.find((el) => el.number === val).isClicked = true;
      //проверка текущего этажа
      const currentStage = JSON.parse(localStorage.getItem("stage"));
      if (currentStage === this.callStage) {
        this.arrivedLift(val);
        return;
      }
    },
    arrivedLift(val) {
      this.stages.find((el) => el.number === val).isClicked = false;
    },
    //Проверка состояния кнопок вызова при обновлении страницы
    checkLiftCallButtonState() {
      if (JSON.parse(localStorage.getItem("stackStages"))) {
        const calledStages = JSON.parse(localStorage.getItem("stackStages"));
        calledStages.forEach((element) => {
          this.stages.find((el) => el.number === element).isClicked = true;
        });
      }
    },
  },
};
</script>

<style></style>
