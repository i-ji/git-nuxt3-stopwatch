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
const emits = defineEmits(["rap"]);

const timer = ref<string>("00:00.000");

let startTime: number;
let timeoutId: number;
let elapsedTime: number = 0;

function countUp() {
  const d = ref<number>(Date.now() - startTime + elapsedTime);
  const date = ref<Date>(new Date(d.value));
  const m = String(date.value.getMinutes()).padStart(2, "0");
  const s = String(date.value.getSeconds()).padStart(2, "0");
  const ms = String(date.value.getMilliseconds()).padStart(3, "0");

  timer.value = `${m}:${s}.${ms}`;

  timeoutId = window.setTimeout(() => {
    countUp();
  }, 10);
}

let disabledStartBtn = ref<boolean>(false);
let disabledStopBtn = ref<boolean>(true);
let disabledResetBtn = ref<boolean>(true);
let disabledRapBtn = ref<boolean>(false);

function setBtnStateInitial() {
  disabledStartBtn.value = false;
  disabledStopBtn.value = true;
  disabledResetBtn.value = true;
  disabledRapBtn.value = true;
}

function setBtnStateRunning() {
  disabledStartBtn.value = true;
  disabledStopBtn.value = false;
  disabledResetBtn.value = true;
  disabledRapBtn.value = false;
}

function setBtnStateStopped() {
  disabledStartBtn.value = false;
  disabledStopBtn.value = true;
  disabledResetBtn.value = false;
  disabledRapBtn.value = true;
}

setBtnStateInitial();

const start = () => {
  setBtnStateRunning();
  startTime = Date.now();
  countUp();
};

const stop = () => {
  setBtnStateStopped();
  clearTimeout(timeoutId);
  elapsedTime += Date.now() - startTime;
};

const reset = () => {
  setBtnStateInitial();
  timer.value = "00:00.000";
  elapsedTime = 0;
};

const rap = () => {
  emits("rap");
};
</script>
