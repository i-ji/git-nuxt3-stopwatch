<template>
  <div>
    <Watch
      @rap="rap"
      @reset="reset"
      @start="start"
      @stop="stop"
      :timer="timer"
      :disabledStartBtn="disabledStartBtn"
      :disabledStopBtn="disabledStopBtn"
      :disabledResetBtn="disabledResetBtn"
      :disabledRapBtn="disabledRapBtn"
    ></Watch>
    <Log :logs="logs"></Log>
  </div>
</template>

<script setup lang="ts">
// timer初期値
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

// buttonのdisabledの管理
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

// start処理
const start = () => {
  setBtnStateRunning();
  startTime = Date.now();
  countUp();
};

// stop処理
const stop = () => {
  setBtnStateStopped();
  clearTimeout(timeoutId);
  elapsedTime += Date.now() - startTime;
};

// rap処理
// let intervalTime: number = 0;
const logs = ref<any[]>([]);
const rap = () => {
  let log = reactive({
    rap: "ラップ",
    time: timer.value,
  });
  logs.value.push(log);
};

// reset処理
const reset = () => {
  // intervalTime = 0;
  setBtnStateInitial();
  timer.value = "00:00.000";
  elapsedTime = 0;
  logs.value = [];
};
</script>
