<template>
  <div>
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
const props = defineProps(["logs"]);
</script>
