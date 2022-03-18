<template>
  <div class="border h-32">
    <div class="relative left-48 flex flex-col gap-y-2">
      <p>{{ stage.number }}</p>
      <div
        @click="callStages"
        class="wrapper cursor-pointer border border-blue-400 w-7 flex justify-center p-1"
        :class="{ isClicked: stage.isClicked }"
      >
        <div
          class="border border-blue-700 w-5 flex justify-center p-1 rounded-full"
          :class="{ isClicked: stage.isClicked }"
        >
          <input :id="stage.number" type="radio" class="hidden" />
          <label
            :for="stage.number"
            class="w-2 h-2 inline-block rounded-full bg-blue-700 cursor-pointer"
            :class="{ isChecked: stage.isClicked }"
          >
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    stage: {
      type: Object,
      required: true,
    },
  },
  methods: {
    callStages() {
      this.$emit("stage", this.stage.number);
    },
  },
};
</script>

<style scoped>
.wrapper:active {
  background-color: rgb(231, 173, 11);
}
.isChecked {
  background-color: orange;
}
.isClicked {
  border: 1px solid orange;
}
</style>
