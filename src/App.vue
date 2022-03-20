<template>
  <div id="app" class="relative">
    <div class="absolute left-10 top-0 right-0 bottom-0 flex gap-x-4">
      <Shaft
        v-for="shaft in shafts"
        :key="shaft.id"
        class="relative"
        :callStage="shaft.stage"
        @arrived="arrivedLift(shaft.id, ...arguments)"
        @stackStages="getStackStages"
      />
    </div>
    <div class="mt-2 flex flex-col-reverse">
      <Stage
        v-for="stage in stages"
        :key="stage.id"
        :stage="stage"
        :shafts="shafts"
        @stage="goToStage"
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
      shafts: [
        { id: 1, isFree: true, stage: 1 },
        { id: 2, isFree: true, stage: 1 },
        { id: 3, isFree: true, stage: 1 },
        { id: 4, isFree: true, stage: 1 },
      ],
      stackStages: [],
    };
  },
  mounted() {
    this.checkLiftCallButtonState();
  },
  methods: {
    goToStage(val) {
      this.stages.find((el) => el.number === val).isClicked = true;
      console.log("goToStage", val);
      const freeShaft = this.shafts.find((el) => el.isFree);
      console.log("freeShaft", freeShaft);
      if (freeShaft === undefined) {
        this.stackStages.push(val);
        return;
      }
      if (this.shafts.find((el) => el.stage === val)) {
        console.log("Этаж занят");
        return;
      }
      freeShaft.stage = val;
      freeShaft.isFree = false;
      //проверка текущего этажа
      // const currentStage = JSON.parse(localStorage.getItem("stage"));
      // if (currentStage === freeShaft.stage) {
      //   this.arrivedLift(val);
      //   return;
      // }
    },
    arrivedLift(id, stage) {
      console.log("arrived lift", id, stage);
      this.shafts.find((el) => el.id === id).isFree = true;
      this.stages.find((el) => el.number === stage).isClicked = false;
      if (this.stackStages.length) {
        this.goToStage(this.stackStages.shift());
      }
    },
    //Проверка состояния кнопок вызова при обновлении страницы
    checkLiftCallButtonState() {
      // if (JSON.parse(localStorage.getItem("stackStages"))) {
      //   const calledStages = JSON.parse(localStorage.getItem("stackStages"));
      //   calledStages.forEach((element) => {
      //     this.stages.find((el) => el.number === element).isClicked = true;
      //   });
      // }
    },
    getStackStages() {
      // const stackStages = arr;
      // console.log(stackStages);
    },
    // checkFreeLift() {
    //   return this.shafts.find((el) => el.isFree === true);
    // },
  },
};
</script>

<style></style>
