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
    <!-- <Log></Log> -->
    <ul class="text-center text-lg w-[320px] mx-auto">
      <li
        v-for="(log, index) in logs"
        :key="log"
        class="mb-2 flex justify-between"
      >
        <span class="font-sans">{{ log.rap }}{{ index + 1 }}</span>
        <span>{{ log.time }}</span>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
const logs = ref<any[]>([]);

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

const rap = () => {
  let log = reactive({
    rap: "ラップ",
    time: "00:01.000",
  });
  logs.value.push(log);
};

const reset = () => {
  setBtnStateInitial();
  timer.value = "00:00.000";
  elapsedTime = 0;
  logs.value = [];
};
</script>
