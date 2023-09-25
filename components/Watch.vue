<template>
  <div class="my-5 mx-auto w-[360px] bg-white p-5 text-center">
    <div class="bg-gray-300 h-40 leading-[160px] text-[48px] mb-5">
      {{ timer }}
    </div>
    <div class="flex justify-between">
      <Button
        btnName="Start"
        @timerAction="start"
        :disabledBtn="disabledStartBtn"
      ></Button>
      <Button
        btnName="Stop"
        @timerAction="stop"
        :disabledBtn="disabledStopBtn"
      ></Button>
      <Button
        btnName="Reset"
        @timerAction="reset"
        :disabledBtn="disabledResetBtn"
      ></Button>
      <Button
        btnName="Rap"
        @timerAction="rap"
        :disabledBtn="disabledRapBtn"
      ></Button>
    </div>
  </div>
</template>

<style>
button:disabled {
  background: #eee;
}
</style>

<script setup lang="ts">
const emits = defineEmits(["start", "stop", "reset", "rap"]);
const props = defineProps([
  "timer",
  "disabledStartBtn",
  "disabledStopBtn",
  "disabledResetBtn",
  "disabledRapBtn",
]);

const start = () => {
  emits("start");
};

const stop = () => {
  emits("stop");
};

const reset = () => {
  emits("reset");
};

const rap = () => {
  emits("rap");
};
</script>
