<template>
  <div>
    <ul>
      <li></li>
    </ul>
  </div>
</template>

<script setup lang="ts">
const logs = ref<any[]>([]);
let log = reactive({
  rap: "ラップ",
  time: "00:01.000",
});
logs.value.push(log);
</script>
