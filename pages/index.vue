<template>
  <div>
    <Watch @rap="rap"></Watch>
    <!-- <Log></Log> -->
    <ul class="bg-blue-400 text-center text-lg">
      <li v-for="log in logs" :key="log" class="mb-2">
        <span>{{ log.rap }}: </span>
        <span>{{ log.time }}</span>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
const logs = ref<any[]>([]);

const rap = () => {
  let log = reactive({
    rap: "ラップ",
    time: "00:01.000",
  });
  logs.value.push(log);
};
</script>
