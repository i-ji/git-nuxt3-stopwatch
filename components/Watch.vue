<template>
  <div class="my-5 mx-auto w-[270px] bg-white p-[15px] text-center">
    <div class="bg-gray-300 h-[120px] leading-[120px] text-[40px] mb-[15px]">
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
    </div>
  </div>
</template>

<style>
button:disabled {
  background: #eee;
}
</style>

<script setup lang="ts">
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

function setBtnStateInitial() {
  disabledStartBtn.value = false;
  disabledStopBtn.value = true;
  disabledResetBtn.value = true;
}

function setBtnStateRunning() {
  disabledStartBtn.value = true;
  disabledStopBtn.value = false;
  disabledResetBtn.value = true;
}

function setBtnStateStopped() {
  disabledStartBtn.value = false;
  disabledStopBtn.value = true;
  disabledResetBtn.value = false;
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
</script>
